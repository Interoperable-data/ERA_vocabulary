# era-vocabulary

This is the repository where the vocabulary (ontology) for the European Union Agency for Railways is being developed and released. The online documentation for this vocabulary is available at https://data-interop.era.europa.eu/era-vocabulary/, and its URI is http://data.europa.eu/949/. All ontology components (classes, properties) use this base URI and are dereferenceable (e.g., http://data.europa.eu/949/Track).

The repository is organised as in the following folders:
* requirements. This folder contains several spreadsheets with requirements written as competency questions, which were originally created for the initial versions of the ERA vocabulary. Currently this list of requirements may be considered incomplete, and it is possible that it will not be maintained in the future.
* governance. This folder contains a description of the main principles that have been followed for the development of the ERA vocabulary, including decisions on URIs, design patterns, update mechanisms, etc.
* era-skos. This folder contains all the SKOS concept schemes (authority tables) that are used as values for some properties in the ERA vocabulary.
* era-shacl. This folder contains all the SHACL shapes derived from the ERA vocabulary and from the validation rules described in the RINF application guide, which allow validating the ERA Knowledge Graph.
* queries. This folder contains several SPARQL queries that can be used to query the ERA knowledge graph.
* public. This folder contains the HTML version of the ERA vocabulary, which is published in the following URL: ttps://data-interop.era.europa.eu/era-vocabulary/

## Issues

We welcome issues and enhancement requests that follow these guidelines:

1. Issues opened in this repository should concern the [ERA vocabulary definitions](https://github.com/Interoperable-data/ERA_vocabulary/issues).
2. Please label your issues using the corresponding version tag. For example, using the label `v3.0.0`.

## Contributing

Please read and respect the [ERA principles](governance/era-principles.md) while contributing to this repository.

For contributions we follow the "fork-and-pull" Git workflow:

1. **Fork** this repository on GitHub.
2. **Clone** the project in your local machine.
3. **Commit** the changes to your own branch.
4. **Push** your changes back up to your own fork.
5. Submit a **Pull request** to the [**dev**](https://github.com/Interoperable-data/ERA_vocabulary/tree/dev) branch so we can review your changes.

NOTE: Make sure to merge the latest "upstream" version before submitting a pull request.

## Validation procedure for user requests
1. Any actor can request improvements to the ERA Vocabulary.
2. The Vocabulary team evaluates if the request is technically sound and its impact in the current deployment of the Vocabulary itself and on the RINF System.
3. ERA evaluates its impact from the business and project management point of view and ultimately approves (or not) the request. 
4. The actor who requested the improvements is informed on when the request will be attended, including an indication of the Vocabulary release number that will include the request.
5. The request, if approved, is implemented by the Vocabulary team.

