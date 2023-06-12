## Introduction to SPARQL
SPARQL is a query language for RDF data, which is used to represent information in the form of subject-predicate-object triples. SPARQL allows you to search for and retrieve specific information from RDF datasets.

### Basic SPARQL syntax
A SPARQL query consists of several components, including prefixes, variables, and triple patterns.

#### Prefixes
Prefixes are used to define namespaces for the predicates used in the query. They can be declared at the beginning of the query using the PREFIX keyword, followed by the prefix and its corresponding namespace URI. For example:
```
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX era: <http://data.europa.eu/949/>
```
This declares two prefixes, `foaf` and `era`, which correspond to the FOAF and ERA namespaces, respectively.
#### Variables
Variables are used to specify which pieces of data you want to retrieve from the dataset. They are denoted by a question mark (?) followed by a variable name, such as `?opName` or `?uopid`.

#### Triple patterns
Triple patterns are used to specify the conditions that must be met for the query to return results. They consist of a subject, predicate, and object, separated by spaces and enclosed in angle brackets. For example:
```
<http://data.europa.eu/949/functionalInfrastructure/operationalPoints/ATAa> era:uopid ?uopid 
```
This triple pattern specifies that the subject is the resource with the URI http://data.europa.eu/949/functionalInfrastructure/operationalPoints/ATAa, the predicate is `era:uopid`, and the object is a variable `?uopid`.

#### Example query
Here's an example SPARQL query that retrieves the uopid(operational point code) and name of all the operational points in an RDF dataset:
```
PREFIX era: <http://data.europa.eu/949/>
SELECT ?uopid ?opName
WHERE { 
    ?op a era:OperationalPoint .
    era:uopid ?uopid .
    era:opName ?opName
}
```
This query declares a prefix for the ERA namespace and specifies that we want to retrieve the values of the variable `?uopid` and `?opName` for all resources that are instances of the `era:OperationalPoint` class.

The WHERE clause specifies the conditions that must be met for the query to return results. In this case, we're looking for resources that have a type `era:OperationalPoint` and a value for the `era:uopid` and `era:opName` properties.

#### Named graphs for resources in the triple store
All the resources published in the triple store MUST be stored in their corresponding named graph, so as to facilitate their management.

The named graph for each type of resources is as follows:

* ERA ontology http://data.europa.eu/949/graph/ontology

* SKOS concept schemes: http://data.europa.eu/949/graph/skos

* Data transformed from RINF XML files: http://data.europa.eu/949/graph/rinf

* Data transformed from the ERATV database: http://data.europa.eu/949/graph/eratv

* Data about infrastructure managers: http://data.europa.eu/949/graph/im

#### Using Graphs
In SPARQL, graphs can be used to organize and segment RDF data into named sets of triples that can be queried independently. Graphs are identified by URIs and can be queried using the GRAPH keyword in a SPARQL query.

To use graphs in SPARQL, follow these steps:

* Define the namespace prefixes that you will be using in the query using the PREFIX keyword.

* Use the SELECT keyword to specify the variables that you want to retrieve in the query.

* Use the GRAPH keyword to specify the named graph that you want to query. You can specify the graph either in the FROM clause, the FROM NAMED clause, or directly in the WHERE clause using the GRAPH keyword.

* Use the WHERE clause to specify the conditions that must be met for the query to return results.

Here's an example SPARQL query that uses a named graph to retrieve triples with the era:opName property set to era:OperationalPoint:
```
PREFIX era: <http://data.europa.eu/949/>
​
SELECT ?opName
WHERE { 
    GRAPH <http://data.europa.eu/949/graph/rinf> {
        ?op a era:OperationalPoint;
        era:opName ?opName.
    }
}
```
In this example, we define a namespace prefixes using the PREFIX keyword: `era` for the ERA vocabulary namespace.

The SELECT keyword is used to specify the `?opName` variable that we want to retrieve in the query.

The WHERE clause is used to specify the conditions that must be met for the query to return results. In this case, we are looking for resources that have the type  `era:OperationalPoint` and an `era:opName` property in the named graph with the URI http://data.europa.eu/949/graph/rinf.

Note that the GRAPH keyword can also be used in combination with the OPTIONAL keyword to retrieve data from a named graph only if it is available, or to join data from multiple named graphs in the same query.

### SPARQL queries on SectionOfLine
##### SectionOfLine
1. SPARQL query from the ERA (European Railway Agency) vocabulary that retrieves information about sections of line:

   This query uses the `era:` prefix to reference properties from the ERA vocabulary, such as `era:SectionOfLine`, `era:imCode`, `era:length`, `era:solNature`, `era:validityStartDate`, and `era:validityEndDate`. The SELECT * statement selects all the properties for each section of line that match the query pattern. 
   
   The WHERE clause specifies the pattern that each result should match. In this case, it selects all instances of `era:SectionOfLine` and retrieves the values for the specified properties.
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX era: <http://data.europa.eu/949/>
SELECT *
WHERE { 
    ?sol a era:SectionOfLine;
    era:imCode ?solImcode;
    era:length ?solLength;
    era:solNature [ skos:prefLabel ?solNature ];
    era:validityStartDate ?validityStartDate;
    era:validityEndDate ?validityEndDate.
} Limit 1
```
This is the result of executing a SPARQL query against https://data-interop.era.europa.eu/endpoint . The query selects all available properties of instances of the `era:SectionOfLine` class that satisfy the given criteria, and limits the result to the first (1) instance.

The result consists of a table with six columns (`sol`, `solImcode`, `solLength`, `solNature`, `validityStartDate`, and `validityEndDate`). The head element lists the variable names, and the results element contains a single result that represents the values of these variables for one particular instance of `era:SectionOfLine`.

In this specific case, the result contains bindings for all variables, indicating the specific values for each property of the selected instance. The `?sol` variable is bound to an URI that identifies the instance, while the other variables are bound to literals that represent the corresponding property values.

For instance, the `solImcode` variable is bound to the literal value "0076", which is the value of the `era:imCode` property of the selected instance. Similarly, the `?solLength` variable is bound to a literal value "3890.0", which is the length of the section of the line. The `?solNature` variable is bound to the literal value "Regular SoL", which is the value of the `skos:prefLabe`l property of the `era:solNature` property of the selected instance. The `?validityStartDate` and `?validityEndDate` variables are bound to date literals representing the start and end dates of the validity period for the instance, respectively.

**XML Output**
```
<sparql
    xmlns="http://www.w3.org/2005/sparql-results#"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.w3.org/2001/sw/DataAccess/rf1/result2.xsd">
    <head>
        <variable name="sol"/>
        <variable name="solImcode"/>
        <variable name="solLength"/>
        <variable name="solNature"/>
        <variable name="validityStartDate"/>
        <variable name="validityEndDate"/>
    </head>
    <results distinct="false" ordered="true">
        <result>
            <binding name="sol">
                <uri>http://data.europa.eu/949/functionalInfrastructure/sectionsOfLine/NO-BN-01_NO0010-01000_NO0210-02010</uri>
            </binding>
            <binding name="solImcode">
                <literal>0076</literal>
            </binding>
            <binding name="solLength">
                <literal datatype="http://www.w3.org/2001/XMLSchema#double">3890.0</literal>
            </binding>
            <binding name="solNature">
                <literal>Regular SoL</literal>
            </binding>
            <binding name="validityStartDate">
                <literal datatype="http://www.w3.org/2001/XMLSchema#date">2018-10-01</literal>
            </binding>
            <binding name="validityEndDate">
                <literal datatype="http://www.w3.org/2001/XMLSchema#date">2028-10-01</literal>
            </binding>
        </result>
    </results>
</sparql>
```
**CSV output**
```
SOL	SOLIMCODE	SOLLENGTH	SOLNATURE	VALIDITYSTARTDATE	VALIDITYENDDATE
http://data.europa.eu/949/functionalInfrastructure/sectionsOfLine/1A_BG16005_BG16010	52	5047.0	Regular SoL	10/12/2016	31/12/2050
JSON Output

{
  "head": {
    "link": [],
    "vars": [
      "sol",
      "solImcode",
      "solLength",
      "solNature",
      "validityStartDate",
      "validityEndDate"
    ]
  },
  "results": {
    "distinct": false,
    "ordered": true,
    "bindings": [
      {
        "sol": {
          "type": "uri",
          "value": "http://data.europa.eu/949/functionalInfrastructure/sectionsOfLine/NO-BN-01_NO0010-01000_NO0210-02010"
        },
        "solImcode": {
          "type": "literal",
          "value": "0076"
        },
        "solLength": {
          "type": "typed-literal",
          "datatype": "http://www.w3.org/2001/XMLSchema#double",
          "value": "3890.0"
        },
        "solNature": {
          "type": "literal",
          "value": "Regular SoL"
        },
        "validityStartDate": {
          "type": "typed-literal",
          "datatype": "http://www.w3.org/2001/XMLSchema#date",
          "value": "2018-10-01"
        },
        "validityEndDate": {
          "type": "typed-literal",
          "datatype": "http://www.w3.org/2001/XMLSchema#date",
          "value": "2028-10-01"
        }
      }
    ]
  }
}
```
#### SOLTrack
This SPARQL query retrieves information about railway tracks in Spain. The prefixes used in the query are:

`skos:` the Simple Knowledge Organization System (SKOS) namespace, used for representing concepts and their relationships.

`era:` the European Union Agency for Railways (ERA) namespace, used for representing railway-related information.

`rdfs:` the RDF Schema namespace, used for representing metadata about resources.

`eu-country:` the European Union namespace for countries.

The query selects the following variables: `?solImcode`, `?solLineNationalId`, `?SOLTrackIdentification`, `?solTrackId`, and `?solTrackDirection`. These variables correspond to the IM code of the Section of Line, the national identifier of the line, the name of the SOL track, the identifier of the SOL track, and the direction of the SOL track.

The query uses the WHERE clause to specify the criteria for selecting the desired information. It looks for Sections of Line (`era:SectionOfLine`) that are located in Spain (`era:inCountry eu-country:ESP`) and have a railway track (`era:track ?solTrack`). It then retrieves the IM code of the Section of Line (`era:imCode ?solImcode`), the national identifier of the line (`era:lineNationalId [rdfs:label ?solLineNationalId]`), the name of the track (`?solTrack rdfs:label ?SOLTrackIdentification`), the identifier of the track (`era:trackId ?solTrackId`), and the direction of the track (`era:trackDirection [skos:prefLabel ?solTrackDirection]`).

Finally, the DISTINCT keyword is used to eliminate duplicates in the result set.
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX era: <http://data.europa.eu/949/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX eu-country: <http://publications.europa.eu/resource/authority/country/>
​
SELECT DISTINCT ?solImcode ?solLineNationalId ?SOLTrackIdentification 
?solTrackId ?solTrackDirection  
WHERE { 
    ?sol a era:SectionOfLine; # SectionOfLine
    era:imCode ?solImcode;
   era:lineNationalId [rdfs:label ?solLineNationalId];
    era:inCountry eu-country:ESP; #Country
    era:track ?solTrack. # SOLTrack
    ?solTrack rdfs:label ?SOLTrackIdentification;
    era:trackId ?solTrackId;
    era:trackDirection [skos:prefLabel ?solTrackDirection].            
}
```
#### SOLTrackParameter

This SPARQL query retrieves specific parameters related to railway lines in Spain. It selects the SectionOfLine entities (`?sol`) and their corresponding SOLTrack entities (`?solTrack`) that are located in Spain (`era:inCountry eu-country:ESP`). Then, it retrieves additional information for each SOLTrack, such as its verification information (`?verificationINF`), demonstration information (`?demonstrationINF`), and its TEN classification (`?tenClassification`), if available.

The OPTIONAL keyword is used to specify that these additional information attributes are not required to be present for each SOLTrack. Therefore, if any of these attributes are missing for a SOLTrack, the query will still retrieve the other available information.
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX era: <http://data.europa.eu/949/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX eu-country: <http://publications.europa.eu/resource/authority/country/>
​
SELECT 
?sol
?solTrack
?verificationINF
?demonstrationINF
?tenClassification
WHERE { 
    ?sol a era:SectionOfLine; # SectionOfLine
    era:inCountry eu-country:ESP; #Country
    era:track ?solTrack. # SOLTrack
    OPTIONAL{   ?solTrack era:verificationINF ?verificationINF.} # 1.1.1.1.1.1
    OPTIONAL{   ?solTrack era:demonstrationINF ?demonstrationINF.} # 1.1.1.1.1.2
    OPTIONAL{   ?solTrack era:tenClassification [skos:prefLabel ?tenClassification]} # 1.1.1.1.2.1 
}
```
#### Tunnel
SPARQL query that retrieves information about tunnels located in Spain from the European Railway Agency (ERA).

The query begins with defining several prefixes: skos, era, rdfs, eu-country, and geosparql. These are used as shortcuts to write the URIs for properties and classes used in the query.

The SELECT clause specifies the variables to be returned by the query: `?tunnelIdentification`, `?imCode`, `?StartLocation`  , and `?endLocation`.

The WHERE clause contains the pattern that the query will match against the data. It starts by identifying the subject of the query as an `era:Tunnel`. Then, it retrieves the `imCode`, `tunnelIdentification`, `startLocation`, and `endLocation` properties of the tunnel, using the `era` namespace. The `geosparql` namespace is used to extract the geographical locations of the start and end of the tunnel, by converting them to WKT format using the `geosparql:asWKT` property. Finally, it filters the results to include only those tunnels located in Spain (`era:inCountry eu-country:ESP`).

The OPTIONAL keyword is used to specify that the following properties are optional for each tunnel: `verificationSRT`, `demonstrationSRT`, `length`, and `crossSectionArea`. If a tunnel has these properties, their values will be returned in the query result, otherwise, they will be left blank.
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX era: <http://data.europa.eu/949/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX eu-country: <http://publications.europa.eu/resource/authority/country/>
PREFIX geosparql: <http://www.opengis.net/ont/geosparql#>
SELECT 
?tunnelIdentification
?imCode
?StartLocation
?endLocation
?verificationSRT
?demonstrationSRT
?length
?crossSectionArea
WHERE { 
    ?tunnel a era:Tunnel;
    era:imCode ?imCode;
    era:tunnelIdentification ?tunnelIdentification;
    era:startLocation [geosparql:asWKT ?StartLocation];
    era:endLocation  [geosparql:asWKT ?endLocation];
    era:inCountry eu-country:ESP.
    OPTIONAL{ ?tunnel era:verificationSRT ?verificationSRT }
    OPTIONAL{ ?tunnel era:demonstrationSRT ?demonstrationSRT }
    OPTIONAL{ ?tunnel era:length ?length}
    OPTIONAL{ ?tunnel era:crossSectionArea ?crossSectionArea} 
}
```
### SPARQL queries on OperationalPoint
#### OperationalPoint
**Example1.**

This SPARQL example retrieves information about operational points (ops) located in countries within the European Union. The SELECT statement specifies which variables to retrieve for each matching result, including the country, the name of the operational point, its unique identifier, its TAF TAP code, the operational point type, its geometry, and its validity dates.

The SELECT statement lists the variables that will be returned in the query results. These variables are:

`?country`: the country where the operational point is located

`?opName`: the name of the operational point

`?uopid`: the unique identifier of the operational point

`?tafTAPCode`: the TafTAPCodeof the operational point

`?opType`: the type of the operational point

`?hasGeometry`: the geometry of the operational point

`?validityStartDate`: the start date of the validity period of the operational point

`?validityEndDate`: the end date of the validity period of the operational point

The WHERE clause is used to specify the conditions that the results must meet. The query retrieves all the ERA operational points (?op) that have the following properties:

`era:opName`: the name of the operational point

`era:uopid`: the unique identifier of the operational point

`era:tafTAPCode`: the TafTAPCodeof code of the operational point

`era:opType`: the type of the operational point

`geosparql:hasGeometry`: the geometry of the operational point

`era:validityStartDate`: the start date of the validity period of the operational point

`era:validityEndDate`: the end date of the validity period of the operational point

`era:inCountry`: the country where the operational point is located

The FILTER clause is used to restrict the results to those where the `?opType` variable has a language tag of "en", indicating that the type is in English.

Finally, the ORDER BY clause sorts the results by country, and the LIMIT clause limits the number of results returned to 500.
```
PREFIX wgs: <http://www.w3.org/2003/01/geo/wgs84_pos#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX era: <http://data.europa.eu/949/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX eu-country: <http://publications.europa.eu/resource/authority/country/>
PREFIX geosparql: <http://www.opengis.net/ont/geosparql#>
SELECT
?country
?opName
?uopid
?tafTAPCode
?opType
?hasGeometry
?validityStartDate
?validityEndDate
WHERE { 
    ?op a era:OperationalPoint;
    era:opName ?opName;
    era:uopid ?uopid;
    era:tafTAPCode ?tafTAPCode;
    era:opType [skos:prefLabel ?opType];
    geosparql:hasGeometry [geosparql:asWKT ?hasGeometry];
    era:validityStartDate ?validityStartDate;
    era:validityEndDate ?validityEndDate;
    era:inCountry ?country. 
    FILTER (lang(?opType) = 'en')
} order by ?country
limit 500
```

**Example2.**

This SPARQL query retrieves information about operational points located in Austria, ordered by their unique identifier. The query selects the following variables for each operational point: `?opName`, `?uopid  ,     ?tafTAPCode` , `?opType, `?`hasGeometry`, `?validityStartDate`, and `?validityEndDate`.

Then, the SELECT statement defines the variables to be retrieved.

In the WHERE clause, the query searches for operational points that have the following properties:

`era:OperationalPoint`: the resource is an instance of era:OperationalPoint.

`era:opName ?opName`: the operational point has a name, which will be returned as opName.

`era:uopid ?uopid`: the operational point has a unique identifier, which will be returned as uopid.

`era:tafTAPCode ?tafTAPCode`: the operational point has a TafTAPCode, which will be returned as tafTAPCode.

`era:opType [skos:prefLabel ?opType]`: the operational point has a type, which will be returned as opType. The skos:prefLabel property is used to retrieve the label of the type in English.

`geosparql:hasGeometry [geosparql:asWKT ?hasGeometry]`: the operational point has a geometry, which will be returned as hasGeometry.

`era:validityStartDate ?validityStartDate`: the operational point has a validity start date, which will be returned as validityStartDate.

`era:validityEndDate ?validityEndDate`: the operational point has a validity end date, which will be returned as validityEndDate.

`era:inCountry eu-country:AUT`: the operational point is located in Austria.

Finally, the FILTER statement is used to ensure that the label of the `opType` property is in English. The results are ordered by `uopid` and limited to 500 results.
```
PREFIX wgs: <http://www.w3.org/2003/01/geo/wgs84_pos#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX era: <http://data.europa.eu/949/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX eu-country: <http://publications.europa.eu/resource/authority/country/>
PREFIX geosparql: <http://www.opengis.net/ont/geosparql#>
SELECT
?opName
?uopid
?tafTAPCode
?opType
?hasGeometry
?validityStartDate
?validityEndDate
WHERE { 
    ?op a era:OperationalPoint;
    era:opName ?opName;
    era:uopid ?uopid;
    era:tafTAPCode ?tafTAPCode;
    era:opType [skos:prefLabel ?opType];
    geosparql:hasGeometry [geosparql:asWKT ?hasGeometry];
    era:validityStartDate ?validityStartDate;
    era:validityEndDate ?validityEndDate;
    era:inCountry eu-country:AUT. 
    FILTER (lang(?opType) = 'en')
} order by ?uopid
limit 500
```
**CSV Output**
```
OPNAME	UOPID	TAFTAPCODE	OPTYPE	HASGEOMETRY	VALIDITYSTARTDATE	VALIDITYENDDATE	
W.Mat.-Altmannsdorf (in Wbf)	ATAa	AT05102	junction	POINT(16.3321575 48.1633296)	28/03/2023	31/12/2099	
Aschbach	ATAb	AT01054	station	POINT(14.7551269 48.0691628)	28/03/2023	31/12/2099	
Abfaltersbach West	ATAbf	AT03877	station	POINT(12.5110857 46.753358)	28/03/2023	31/12/2099	
AB (Awanst) - Abf_A1	ATAbf A1	AT05104	junction	POINT(12.6471485 46.780965)	28/03/2023	31/12/2099	
```
#### OPTrack
This SPARQL query retrieves the operational tracks associated with operational points located in France. It selects three variables for each operational track: `?opTrack`, `?OPTrackIdentification`, and `?TrackImCod`e.

The query pattern starts by specifying the operational point (`?op`) and its associated properties (`era:inCountry` and `era:opName`). It then selects the operational track (`?opTrack`) associated with the operational point through the era:track property. For each track, it selects its identification code (`?OPTrackIdentification`) and its IM (`?Infrastructure Manager`) code (`?TrackImCode`). The order by `?uopid` statement sorts the results by the `?uopid` variable.
```
PREFIX wgs: <http://www.w3.org/2003/01/geo/wgs84_pos#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX era: <http://data.europa.eu/949/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX eu-country: <http://publications.europa.eu/resource/authority/country/>
PREFIX geosparql: <http://www.opengis.net/ont/geosparql#>
SELECT
?opTrack
?OPTrackIdentification
?TrackImCode
WHERE { 
    ?op a era:OperationalPoint;
    era:inCountry eu-country:FRA;
    era:opName ?opName;
    era:uopid ?uopid;
    era:track ?opTrack.
    ?opTrack era:imCode ?TrackImCode;
    era:trackId ?OPTrackIdentification.
    
} order by ?uopid
```
#### OPTrackParameter
This is a SPARQL query that selects specific properties of tracks associated with Operational Points located in Austria. The prefixes used in this query are `skos`, `era`, and `eu-country`, which are used to define URIs for various properties used in the query.

The properties selected in this query are as follows:

`OPTrackIdentification`: the ID of the track

`TrackImCode`: the IM Code of the track

`verificationINF`: optional property

`demonstrationINF`: optional property

`tenClassification`: optional property

`lineCategory`: optional property indicating the category of the track

`freightCorridor`: optional property indicating the freight corridor the track belongs to

`gaugingProfile`: optional property indicating the gauging profile of the track

`gaugingCheckLocation`: optional property on the track

`gaugingTransversalDocument`: optional property

`wheelSetGauge`: optional property indicating the gauge of the wheel set used on the track

The query uses the WHERE clause to specify the conditions that must be met in order for a track to be selected. In this case, the query looks for Operational Points located in Austria that have associated tracks. The OPTIONAL keyword is used to indicate that the properties selected are optional and do not need to be present for the track to be included in the results. Finally, the limit keyword is used to limit the number of results returned to 500.
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX era: <http://data.europa.eu/949/>
PREFIX eu-country: <http://publications.europa.eu/resource/authority/country/>
SELECT
?OPTrackIdentification
?TrackImCode
?verificationINF
?demonstrationINF
?tenClassification
?lineCategory
?freightCorridor
?gaugingProfile
?gaugingCheckLocation
?gaugingTransversalDocument
?wheelSetGauge
WHERE { 
    ?op a era:OperationalPoint;
    era:inCountry eu-country:AUT;
    era:opName ?opName;
    era:uopid ?uopid;
    era:track ?opTrack.
    ?opTrack era:imCode ?TrackImCode;
    era:trackId ?OPTrackIdentification.
    OPTIONAL { ?opTrack era:verificationINF  ?verificationINF}
    OPTIONAL { ?opTrack era:demonstrationINF ?demonstrationINF}
    OPTIONAL { ?opTrack era:tenClassification [skos:prefLabel ?tenClassification]}
    OPTIONAL { ?opTrack era:lineCategory [skos:prefLabel ?lineCategory]}
    OPTIONAL { ?opTrack era:freightCorridor [skos:prefLabel ?freightCorridor]}
    OPTIONAL { ?opTrack era:gaugingProfile [skos:prefLabel ?gaugingProfile]}
    OPTIONAL { ?opTrack era:gaugingCheckLocation ?gaugingCheckLocation}
    OPTIONAL { ?opTrack era:gaugingTransversalDocument ?gaugingTransversalDocument}
    OPTIONAL { ?opTrack era:wheelSetGauge ?wheelSetGauge}
} limit 500
```
#### OPSiding
This SPARQL query retrieves information about sidings from the ERA (European Union Agency for Railways) . The prefixes used in the query are:

`skos:` prefix for the Simple Knowledge Organization System, used for the preferred label of some properties

`era:` prefix for the ERA ontology

`eu-country:` prefix for the list of European Union countries

The SELECT * statement indicates that all information available for each siding should be retrieved. The WHERE clause sets the conditions for the information retrieval.

The query finds all instances of the class era:Siding and retrieves the values of the following properties for each instance:

`era:imCode`: the identification code of the siding

`era:sidingId`: the ID of the siding

`era:tenClassification`: the classification of the siding

`era:verificationINF`: the verification information of the siding, if available

`era:demonstrationINF`: the demonstration information of the siding, if available

`era:length`: the length of the siding, if available

`era:gradient`: the gradient of the siding, if available

`era:minimumHorizontalRadius`: the minimum horizontal radius of the siding, if available

`era:MinimumVerticalRadiusCrest`: the minimum vertical radius at crest of the siding, if available

`era:MinimumVerticalRadiusHollow`: the minimum vertical radius at hollow of the siding, if available

`era:hasToiletDischarge`: Indication whether exists an installation for toilet discharge, if available

`era:hasExternalCleaning`:Indication whether exists an installation for external cleaning facility (fixed installation for servicing trains) as defined in INF TSIs, if available

`era:hasWaterRestocking`: Indication whether exists an installation for water restocking (fixed installation for servicing trains) as defined in INF TSIs, if available

`era:hasRefuelling`: Indication whether exists an installation for refuelling (fixed installation for servicing trains) as defined in INF TSIs, if available

`era:hasSandRestocking`: Indication whether an installation for sand restocking exists (fixed installation for servicing trains), if available

`era:hasElectricShoreSupply`: Indication whether an installation for electric shore supply exists (fixed installation for servicing trains), if available

`era:maxCurrentStandstillPantograph`: Indication of the maximum allowable train current at standstill expressed in amperesif available

The OPTIONAL keyword is used to specify that certain properties are not required for the information retrieval. Finally, the limit 500 statement limits the number of results to 500.
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX era: <http://data.europa.eu/949/>
PREFIX eu-country: <http://publications.europa.eu/resource/authority/country/>
SELECT
*
WHERE { 
    ?sid a era:Siding;
    era:imCode ?sidingImCode;
    era:sidingId ?sidingId.
    OPTIONAL { ?sid era:tenClassification [skos:prefLabel ?tenClassification]}
    OPTIONAL { ?sid era:verificationINF ?verificationINF}
    OPTIONAL { ?sid era:demonstrationINF ?demonstrationINF}      
    OPTIONAL { ?sid era:length  ?length }
    OPTIONAL { ?sid era:gradient ?gradient}
    OPTIONAL { ?sid era:minimumHorizontalRadius ?minimumHorizontalRadius}
    OPTIONAL { ?sid era:MinimumVerticalRadiusCrest ?MinimumVerticalRadiusCrest}
    OPTIONAL { ?sid era:MinimumVerticalRadiusHollow ?MinimumVerticalRadiusHollow}
    OPTIONAL { ?sid era:hasToiletDischarge ?hasToiletDischarge}
    OPTIONAL { ?sid era:hasExternalCleaning ?hasExternalCleaning}
    OPTIONAL { ?sid era:hasWaterRestocking ?hasWaterRestocking}
    OPTIONAL { ?sid era:hasRefuelling ?hasRefuelling}
    OPTIONAL { ?sid era:hasSandRestocking ?hasSandRestocking}
    OPTIONAL { ?sid era:hasElectricShoreSupply ?hasElectricShoreSupply}
    OPTIONAL { ?sid era:maxCurrentStandstillPantograph  ?maxCurrentStandstillPantograph}
  
} limit 500
```
### SOLTrack notYetAvailable and notApplicable
This SPARQL query is retrieving information about railway tracks in Austria, specifically their track identification, not yet available and not applicable property.

The query selects the following variables:

`?track`: the railway track

`?SOLTrackIdentification`: the track identification of the section of line

`?notYetAvailable`: notYetAvailable property, expressed as a label

`?notApplicable`: notApplicable property, expressed as a label

The WHERE clause specifies the following conditions:

The `?sol` variable represents a SectionOfLine object that is located in Austria (`era:inCountry eu-country:AUT`) and has a track associated with it.

The `?track` variable represents the track associated with the `?sol` object and has a track identification (`era:trackId`) that is assigned to the `?SOLTrackIdentification` variable.

The track may be marked as not yet available or not applicable, which are represented as objects with an `rdfs:label` property that are assigned to the `?notYetAvailable` and `?notApplicable` variables, respectively.

The query returns only distinct results and sorts them in ascending order by `?SOLTrackIdentification`.
```
PREFIX era: <http://data.europa.eu/949/>
PREFIX eu-country: <http://publications.europa.eu/resource/authority/country/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
​
SELECT DISTINCT ?track ?SOLTrackIdentification ?notYetAvailable ?notApplicable
WHERE {
    ?sol a era:SectionOfLine;
    era:inCountry eu-country:AUT;
    era:track ?track.
    ?track era:trackId ?SOLTrackIdentification;
    era:notYetAvailable [rdfs:label ?notYetAvailable];
    era:notApplicable [rdfs:label ?notApplicable]. 
}order by ?SOLTrackIdentification
```
### OPTrack notYetAvailable and notApplicable
This SPARQL query retrieves information about tracks located in Austria .

The query selects the following variables:

`?track`: the track associated with the operational point

`?OPTrackIdentification`: the identification code of the track

`?notYetAvailable`: not yet available property

`?notApplicable`: not applicable property

The query specifies the following conditions:

The operational point (`?o`p) must belong to Austria (`era:inCountry eu-country:AUT`)

The operational point must be associated with a track (`era:track ?track`)

The identification code of the track (`?track era:trackId ?OPTrackIdentification`) must be retrieved

The label of the not yet available parameter of the track (`era:notYetAvailable [rdfs:label ?notYetAvailable]`) must be retrieved (if it exists)

The label of the not applicable parameter of the track (`era:notApplicable [rdfs:label ?notApplicable]`) must be retrieved (if it exists)

Finally, the results are ordered by the identification code of the track (order by `?OPTrackIdentification`). The DISTINCT keyword ensures that only unique results are returned.
```
PREFIX era: <http://data.europa.eu/949/>
PREFIX eu-country: <http://publications.europa.eu/resource/authority/country/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
​
SELECT DISTINCT ?track ?OPTrackIdentification
?notYetAvailable 
?notApplicable
WHERE {
    ?op a era:OperationalPoint;
    era:inCountry eu-country:AUT;
    era:track ?track.
    ?track era:trackId ?OPTrackIdentification;
    era:notYetAvailable [rdfs:label ?notYetAvailable];
    era:notApplicable [rdfs:label ?notApplicable]. 
}order by ?OPTrackIdentification
```
### General Examples
* This SPARQL query retrieves data from an RDF dataset that contains information about operational points in Austria. The query selects several properties of these operational points, including their name, type, validity start and end dates, latitude, longitude, track identification, and other properties related to the track.
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX geosparql: <http://www.opengis.net/ont/geosparql#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX wgs: <http://www.w3.org/2003/01/geo/wgs84_pos#>
PREFIX era: <http://data.europa.eu/949/>
PREFIX eu-country: <http://publications.europa.eu/resource/authority/country/>
​
SELECT 
?opName
?opType
?validityStartDate
?validityEndDate
?latitude
?longitude
?uopid
?OPTrackIdentification
?OPTrackIMCode
?verificationINF
?demonstrationINF
?tenClassification
?lineCategory
?freightCorridor
?gaugingProfile
?gaugingCheckLocation
?gaugingTransversalDocument
?wheelSetGauge
WHERE { 
    ?op a era:OperationalPoint;
    era:opName ?opName;
    era:opType [skos:prefLabel ?opType];
    era:uopid ?uopid;
    era:inCountry eu-country:AUT; # Country
    era:validityStartDate ?validityStartDate;
    era:validityEndDate ?validityEndDate;
    era:tafTAPCode ?tafTAPCode;
    geosparql:hasGeometry [ wgs:lat ?latitude];
    geosparql:hasGeometry [ wgs:long ?longitude];                           
    era:track ?track.
    ?track era:trackId ?OPTrackIdentification;
    era:imCode ?OPTrackIMCode.
    OPTIONAL{?track era:verificationINF ?verificationINF.} # 1.2.1.0.1.1 
    OPTIONAL{?track era:demonstrationINF ?demonstrationINF.} # 1.2.1.0.1.2
    OPTIONAL{?track era:tenClassification [skos:prefLabel ?tenClassification]} # 1.2.1.0.2.1
    OPTIONAL{?track era:lineCategory [skos:prefLabel ?lineCategory] } # 1.2.1.0.2.2       
    OPTIONAL{?track era:freightCorridor [skos:prefLabel ?freightCorridor] } # 1.2.1.0.2.3
    OPTIONAL{?track era:gaugingProfile [skos:prefLabel ?gaugingProfile]} # 1.2.1.0.3.4      
    OPTIONAL{?track era:gaugingCheckLocation  ?gaugingCheckLocation.}  # 1.2.1.0.3.5
    OPTIONAL{?track era:gaugingTransversalDocument ?gaugingTransversalDocument.} # 1.2.1.0.3.6        
    OPTIONAL{?track era:wheelSetGauge ?wheelSetGauge.}      # 1.2.1.0.4.1
} ORDER BY ?uopid
LIMIT 5000
```
**Example of JSON output**
```
{
  "head": {
    "link": [
      
    ],
    "vars": [
      "opName",
      "opType",
      "validityStartDate",
      "validityEndDate",
      "latitude",
      "longitude",
      "uopid",
      "OPTrackIdentification",
      "OPTrackIMCode",
      "verificationINF",
      "demonstrationINF",
      "tenClassification",
      "lineCategory",
      "freightCorridor",
      "gaugingProfile",
      "gaugingCheckLocation",
      "gaugingTransversalDocument",
      "wheelSetGauge"
    ]
  },
  "results": {
    "distinct": false,
    "ordered": true,
    "bindings": [
      {
        "opName": {
          "type": "literal",
          "value": "W.Mat.-Altmannsdorf (in Wbf)"
        },
        "opType": {
          "type": "literal",
          "xml:lang": "en",
          "value": "junction"
        },
        "validityStartDate": {
          "type": "typed-literal",
          "datatype": "http://www.w3.org/2001/XMLSchema#date",
          "value": "2023-03-28"
        },
        "validityEndDate": {
          "type": "typed-literal",
          "datatype": "http://www.w3.org/2001/XMLSchema#date",
          "value": "2099-12-31"
        },
        "latitude": {
          "type": "typed-literal",
          "datatype": "http://www.w3.org/2001/XMLSchema#double",
          "value": "48.1633"
        },
        "longitude": {
          "type": "typed-literal",
          "datatype": "http://www.w3.org/2001/XMLSchema#double",
          "value": "16.3322"
        },
        "uopid": {
          "type": "literal",
          "value": "ATAa"
        },
        "OPTrackIdentification": {
          "type": "literal",
          "value": "1"
        },
        "OPTrackIMCode": {
          "type": "literal",
          "value": "0081"
        },
        "tenClassification": {
          "type": "literal",
          "value": "Part of the TEN-T Core Freight Network"
        },
        "lineCategory": {
          "type": "literal",
          "value": "P6"
        },
        "freightCorridor": {
          "type": "literal",
          "value": "Baltic-Adriatic RFC (RFC 5)"
        },
        "gaugingProfile": {
          "type": "literal",
          "value": "G2"
        }
      }
    ]
  }
}
```
* This SPARQL query is used to retrieve the distinct sections of lines in each country, along with the English label of the country. It uses two prefixes to define the namespaces of the properties used in the query.

The first prefix, `skos`, refers to the Simple Knowledge Organization System (SKOS) vocabulary, which provides a standard way to represent and use knowledge organization systems such as thesauri, classification schemes, and taxonomies. The `skos:prefLabel` property is used to retrieve the preferred label of a country.

The second prefix, `era`, refers to the European Railway Agency ontology. The `era:SectionOfLine` and `era:inCountry` properties are used to retrieve the section of line and its corresponding country.

The query specifies two separate GRAPH clauses to access different named graphs in the data. The first GRAPH clause is used to retrieve the section of line and its corresponding country from the "rinf" named graph. The second GRAPH clause is used to retrieve the English label of the country from the "countries" named graph.

Finally, the ORDER BY clause is used to sort the results by the `?Country` variable in ascending order.
```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX era: <http://data.europa.eu/949/>
SELECT DISTINCT ?SectionOfLine ?Country  ?label
WHERE {
    GRAPH <http://data.europa.eu/949/graph/rinf>{
        ?SectionOfLine a era:SectionOfLine;
        era:inCountry ?Country.
    }
    GRAPH <http://data.europa.eu/949/graph/countries>{
        ?Country skos:prefLabel ?label
        FILTER(LANG(?label) = "en")
    }
} ORDER BY ?Country
```
### How to use SPARQL with Postman
To use SPARQL with Postman follow these steps:

1. Install Postman: Download and install Postman from the official website (https://www.postman.com/downloads/) if you have not done it yet.
Set up a SPARQL endpoint:

   Ensure you have access to a SPARQL endpoint that you want to query. This can be a public SPARQL endpoint or your own SPARQL server.
1. Create a new request in Postman: Open Postman and create a new HTTP request by clicking on the "New" button in the top left corner. Choose the request method as "GET": 
(images-documentation/picture1.png)

1. Set the GET request URL: In the URL field, enter the URL of your SPARQL endpoint. For example, if your SPARQL endpoint is https://linked.ec-dataplatform.eu/sparql, enter that URL.
1. Set the GET request headers: Click on the "Headers" tab below the request URL field:
   * Content-Type: application/x-www-form-urlencoded
   * Accept: application/sparql-results+json (This header specifies that you want to receive JSON-formatted SPARQL results. Adjust this header if you prefer a different format.) (images-documentation/picture2.png)

1. Configure the request parameters: Click on the "Params" tab below the URL field.  Add a key-value pair where the key is "query" and the value is your SPARQL query. For example:
 (images-documentation/picture6.png)
1. Send the request: Click the "Send" button to send the SPARQL query to the endpoint.
1. View the response: Postman will display the response received from the SPARQL endpoint. The response will contain the results of your query in the specified format (JSON in our example). You can analyze and process the results as needed.
(images-documentation/picture8.png)

That's it! You have successfully used Postman to send a SPARQL query to a SPARQL endpoint and received the results. You can modify the query, headers, and other settings as per your requirements.
