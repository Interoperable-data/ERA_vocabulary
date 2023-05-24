# era-vocabulary

This is the repository where the vocabulary (ontology) for the European Union Agency for Railways is begin developed.

The online documentation for this vocabulary is available at https://data-interop.era.europa.eu/era-vocabulary/, and its URI is http://data.europa.eu/949/. All ontology components (classes, properties) use this base URI and are dereferenceable (e.g., http://data.europa.eu/949/Track).

The vocabulary documentation is built using the [Widoco](https://github.com/dgarijo/Widoco) toolset.

## Documentation generation process

The documentation generation process cannot be fully automated since there are some dependencies in some of the generated HTML files that need to be dealt with, as explained below. Scripts have been generated in order to help in this process.

### Regeneration of the WIDOCO-based Documentation

* Make sure that we have the latest version of the ontology in our local folder (git pull)
* In the **era-widoco.conf** file, change attributes related to the ontology version number, previous versions, date of release, etc. This is not extremely relevant, since manual editing of the final index-en.html will be necessary afterwards, but it is useful to keep control of the documentation that is generated.
* **Run ./widoco-script.sh**. This will do the following steps for you 
  * Rename public/doc folder into public/doc-old
  * Run the RINF-adapted Widoco according to the configuration file and ontology file.
  * Copy public/doc-old/index-en.html into public/doc/index-en.html (replacing the file). This is needed because the old index-en.html has already several additions that do not come with the standard Widoco release: logos, a special way of presenting the ontology and several links to additional javascript files for the representation of Turtle snippets.
  * Move /public/doc-old/resources to /public/doc/resources, replacing everything (only to be rechecked if a new Widoco version is used and some Javascript file has been updated).
  * Move the following files from public/doc-old/sections/ to public/doc/sections 
    * abstract-en.html
    * description-en.html
    * introduction-en.html
    * references-en.html
  * Remove public/doc/readme.md
  * Remove the public/doc-old folder
* **Edit /public/doc/index-en.html with updated references to version numbers, release dates, etc.** (the same as done before in era-widoco.conf)

### Preparation of the SKOS

* **Run ./generate-SKOS.sh**. This will regenerate the era-skos.ttl file from the content of the era-skos folder.
* **cp ./era-skos.ttl /public/doc/**
* **rm /public/doc/skos/*.ttl**. This will delete the SKOS files from the folder, except for public/doc/skos/index.html.
* **cp -r era-skos /public/doc/skos**. This will copy the content of the repository era-skos folder into /public/doc/skos
* **Edit /public/doc/skos/index.html**, to change the version if needed, and also if there is any change in the SKOS concept schemes that are made available.

### Preparation of the SHACL file

* **Run ./generate-SHACL.sh**. This will regenerate the era-shapes.ttl file from the content of the era-shacl folder.
* **cp ./era-shapes.ttl /public/doc/**
* **rm /public/doc/shacl/*.ttl**. This will delete the SHACL files from the folder.
* **cp -r era-shacl /public/doc/shacl**. This will copy the content of the repository era-shacl folder into /public/doc/shacl

### Final deployment (in dev and prod)

Once everything (ontology files, SKOS files and documentation) are ready and pushed into the repository, a new pipeline will be created, where several manual operations need to be done. It is recommended to do this first in the dev environment (dev branch) and once tested do the same by doing a merge request into the prod branch and running again the same set of actions.

* Go to the latest pipeline (https://git.fpfis.tech.ec.europa.eu/datateam/ERA/era-vocabulary/-/pipelines/) and select the latest one.
* Run the Build job. This will create the corresponding Docker image
* Run the Upgrade job. This will modify the corresponding files in the ERA-infra repository, so that the new docker image with the vocabulary is used. As a result, the new ontology documentation should be available at https://dev.ld4rail.fpfis.tech.ec.europa.eu/era-vocabulary/ (for the dev environment) and at https://data-interop.era.europa.eu/era-vocabulary/ (for the prod environment)
* Run the upload-to-s3 job. This will generate the corresponding nquads for the SKOS files and the ontology, and upload them into the corresponding S3 buckets that are used to feed the dev and prod triple stores. Pending: generate these nquads as repository artefacts, so that they can be downloaded as well.

### Prepare the release/tag

If everything is successful and a new version is now available, a tag should be generated in the repository with the corresponding diff information.

## Issues

We welcome issues and enhancement requests that follow these guidelines:

1. Issues opened in this repository should concern the [ERA vocabulary definitions](https://git.fpfis.tech.ec.europa.eu/datateam/ERA/era-vocabulary/-/issues).
2. Please label your issues using the corresponding version tag. For example, using the label `v2.6.0`.

## Contributing

For contributions we follow the "fork-and-pull" Git workflow:

1. **Fork** this repository on GitHub.
2. **Clone** the project in your local machine.
3. **Commit** the changes to your own branch.
4. **Push** your changes back up to your own fork.
5. Submit a **Pull request** to the [**dev**](https://github.com/julianrojas87/era-vocabulary/tree/dev) branch so we can review your changes.

NOTE: Make sure to merge the latest "upstream" version before submitting a pull request.

## Deployment of the vocabulary documentation

For our deployment purposes, the vocabulary documentation is deployed using Docker (hence it may also be used to deploy the vocabulary documentation in any other server). We use a [multi-stage approach](https://docs.docker.com/develop/develop-images/multistage-build/) to build a container that publishes a build of this Web application via a NGINX instance.

To deploy this container follow these steps:

1. Make sure to have a recent version of [Docker](https://docs.docker.com/engine/install/) installed.
2. Build the Docker image:

   ```bash
   docker build -t era-vocabulary ./
   ```
3. Start the application:

   ```bash
   docker run -p ${PORT}:80 era-vocabulary
   ```

   Replace `${PORT}` for the TCP port where you want to run the application. Once the container is running you can access these resources:
   * The vocabulary documentation at `http://localhost:${PORT}/era-vocabulary/index-en.html`.
   * The reference data at `http://localhost:${PORT}/era-vocabulary/era-skos/index.html`.
