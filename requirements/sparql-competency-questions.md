# Competency questions #
## CQ 1 ##
Which are the distinct values for a particular parameter P of a track in member state C?

* P: minimum radius of horizontal curve
* C: France

```
#Q1. Which are the distinct values for a particular parameter P of a track in member state C?
#P: minimum radius of horizontal curve; C: France

PREFIX era:  <http://data.europa.eu/949/>
PREFIX country: <http://publications.europa.eu/resource/authority/country/>

select distinct ?value where
{
?t era:minimumHorizontalRadius ?value .
?t a era:Track .
?s a era:SectionOfLine .
?s era:track ?t .
?s era:inCountry country:FRA
}
```
## CQ 2 ##
How many tunnels are there per member state?
```
#Q2. How many tunnels are there per member state?
PREFIX era:  <http://data.europa.eu/949/>

select ?country count(distinct *) as ?numberTunnels where
{
?t a era:Tunnel .
?t era:inCountry ?country .
}
GROUP BY ?country
```
## CQ 3 ##
What is the tunnel length per member state?
```
#Q3. What is the tunnel length per member state?
PREFIX era:  <http://data.europa.eu/949/>

select ?country round(xsd:decimal(SUM(?length)/1000) as ?totalLengthKm where
{
?t a era:Tunnel .
?t era:length ?length .
?t era:inCountry ?country .
}
GROUP BY ?country
```

## CQ 4 ##
What is the railway length per member state? (takes into account sections of lines and tunnels)

```
#Q4. What is the railway length per member state? (takes into account sections of lines and tunnels)
PREFIX era: <http://data.europa.eu/949/>
PREFIX dc: <http://purl.org/dc/elements/1.1/>

SELECT DISTINCT ?id ?inCountry (round(xsd:decimal(SUM(?length)/1000)) AS ?totalLengthKm)
WHERE {
  ?element a ?y .  # currently includes sections of line and tunnels
  ?element era:length ?length .
  ?element era:inCountry ?inCountry.
  ?inCountry dc:identifier ?id
}
GROUP BY ?inCountry

```
## CQ 5 ##
What are the platform lengths in ascending order? (What is the variety of platform lengths in Eurrope?
```
#Q5. What are the platform lengths in ascending order? (What is the variety of platform lengths in Eurrope?
PREFIX era:  <http://data.europa.eu/949/>

select distinct ?platformLength
{
?p a era:Platform .
?p era:length ?platformLength .
}
ORDER BY asc(?platformLength)
```

## CQ 6 ##
What are the parameters of a certain entity E in the vocabulary 

* E: Operational point

```
#Q6. What are the parameters of a certain entity E in the vocabulary 
#E: Operational point

PREFIX era:  <http://data.europa.eu/949/>

select distinct ?parameter
{
?s a era:OperationalPoint .
?s ?parameter ?o
FILTER STRSTARTS(STR(?parameter),"http://data.europa.eu/949/")
}
```

## CQ 7 ##
Which are non TSI compliant legacy signalling systems existent in the different countries?

```
#Q7. Which are non TSI compliant legacy signalling systems existent in the different countries?

PREFIX era:  <http://data.europa.eu/949/>

select  distinct ?country ?track   WHERE
{
?s a era:SectionOfLine .
?s era:track ?track .
?track era:hasTSITrainDetection ?d .
?s era:inCountry ?country .
FILTER (?d = "false"^^xsd:boolean)
}
ORDER BY ASC(?country)
```
## CQ 8 ##
What are the number of tons per track?
```
#Q8. What are the number of tons per track?

PREFIX era:  <http://data.europa.eu/949/>

select  ?track ?tons   WHERE
{
?track a era:Track .
?track era:contactLineSystem ?c .
?c era:minAxleLoadVehicleCategory ?tons
}
```

## CQ 9 ##
Which are the energy supply system values (voltage frequency) per SOL per member state?
```
#Q9. Which are the energy supply system values (voltage frequency) per SOL per member state?

PREFIX era:  <http://data.europa.eu/949/>

select distinct ?country ?voltageFrequency

{
?s a era:SectionOfLine .
?s era:track ?t .
?s era:inCountry ?country .
?t era:contactLineSystem ?c .
?c era:energySupplySystem ?voltageFrequency .
}
ORDER BY ASC(?country)
```
## CQ 10 ##
What is the network of an infrastructure manager IM?

```
#Q10. What is the network of an infrastructure manager IM?

PREFIX era:  <http://data.europa.eu/949/>

 select ?IMCode round(xsd:decimal(SUM(?length)/1000)) as ?totalLength 
       count(?section) as ?totalSections 
       count(distinct ?opPoint) as ?totalOpPoints
{
?section a era:SectionOfLine .
?section era:imCode ?IMCode .
?section era:length ?length .
?opPoint a era:OperationalPoint .
?section ?p ?opPoint .
}
GROUP BY ?IMCode
```

## CQ 12 ##
Which are the diesel vehicle types?

```
#Q12. Which are the diesel vehicle types?

PREFIX era:  <http://data.europa.eu/949/>

select ?vehicleTypeLabel
{
?vt a era:VehicleType .
?vt rdfs:label ?vehicleTypeLabel .
FILTER REGEX(?vehicleTypeLabel,"diesel","i")
}
```

## CQ 13 ##
Which are the tracks with a contact line system type that is not electrified

```
Q13. Which are the tracks with a contact line system type that is not electrified

PREFIX era:  <http://data.europa.eu/949/>

select distinct ?track WHERE
{
?track a era:Track .
?track era:contactLineSystem ?c .
?c era:contactLineSystemType ?type .
?type skos:prefLabel ?label . 
FILTER (?label = "Not electrified")
}
```

## CQ 14 ##
What is the track length of  Trans-European Transport Network (TEN-T) lines per member state?

```
#Q12. What is the track length of  Trans-European Transport Network (TEN-T) lines per member state?

PREFIX era:  <http://data.europa.eu/949/>

select ?country round(xsd:decimal(SUM(?length)/1000)) as ?totalLength WHERE
{    
?section a era:SectionOfLine .
?section era:inCountry ?country .
?section era:track ?t .
?t era:tenClassification ?c .
?c skos:prefLabel ?l .
?section era:length ?length .
FILTER (STRENDS(?l,"Core Freight Network") || STRENDS(?l,"Core Comprehensive Network") || STRENDS(?l,"Core Passenger Network"))
}
GROUP BY ?country
```



## CQ 16 
How many tracks are equipped with GSM-r per member state (rate of deployment?

```
#Q16. How many tracks are equipped with GSM-r per member state (rate of deployment?

PREFIX era:  <http://data.europa.eu/949/>

select ?country count(?t) as ?numberTracks WHERE
{
?s a era:SectionOfLine .
?s era:track ?t .
?s era:inCountry ?country .
?t era:gsmRNoCoverage ?v .
FILTER (?v = "false"^^xsd:boolean)
}
GROUP BY ?country

```
## CQ 18
Which are the tunnels in Belgium that are not category (rolling stock fire category)  A nor B?
```
#Q18. which are the tunnels in Belgium that are not category (rolling stock fire category)  A nor B?

PREFIX era:  <http://data.europa.eu/949/>

select ?tunnel ?r WHERE
{
?tunnel a era:Tunnel .
?tunnel era:inCountry ?country .
?tunnel era:rollingStockFireCategory ?r
FILTER (STRENDS(STR(?r),"none") && STRENDS(STR(?country),"BEL"))
}
```
## CQ 19 ##
How many countries are using local and non-international Gauges (gauging profile)?
```
#Q19. How many countries are using local and non-international Gauges (gauging profile)? 

PREFIX era:  <http://data.europa.eu/949/>

select count(distinct ?country)  as ?numberCountries WHERE
{    
?section a era:SectionOfLine .
?section era:inCountry ?country .
?section era:track ?t .
?t era:gaugingProfile ?g .
FILTER REGEX(?g,"(G1|GA|GB|GB1|GB2|GC)")
}
```


## CQ 20
What are the available values for the GSM-R version per member state?
```
#Q20. What are the available values for the GSM-R version per member state?

PREFIX era:  <http://data.europa.eu/949/>

select distinct ?country ?gsmrVersion  WHERE
{
?s a era:SectionOfLine .
?s era:track ?t .
?s era:inCountry ?country .
?t era:gsmRVersion ?gsmrVersion
}
ORDER BY ASC(?country)
```

## CQ 21
What are the available ETCS levels  per member state?
```
#Q21. What are the available ETCS levels per member state?

PREFIX era:  <http://data.europa.eu/949/>

select  distinct ?country ?etcsLevel   WHERE
{
?s a era:SectionOfLine .
?s era:track ?t .
?s era:inCountry ?country .
?t era:etcsLevel ?etcsLevel
}
ORDER BY ASC(?country)
```
## CQ 21a
What are the available ETCS baselines  per member state?`
```
#Q21a. What are the available ETCS baselines  per member state?

PREFIX era:  <http://data.europa.eu/949/>

select  distinct ?country ?etcsBaseline   WHERE
{
?s a era:SectionOfLine .
?s era:track ?t .
?t era:etcsLevel ?e .
?e era:etcsBaseline ?etcsBaseline .
?s era:inCountry ?country .
}
ORDER BY ASC(?country)
```







