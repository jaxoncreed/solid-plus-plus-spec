# RDF Dump Support

Pods can optionally support raw [rdf](https://www.w3.org/RDF/), a w3c standard for defining linked data. In this document we outline the interface used to extract a raw data dump of a pod's graph in RDF form.

**Note** Alternatively a container and resource structure can be considered rather than just an RDF dump, however this isn't as strictly required when compared to [linked data platforms](https://www.w3.org/TR/ldp/) because the Solid++ standard is not concerned with serving static files. Instead Solid++ relies on separate CDNs. An additional problem with a container and resource structure is that the path to a resource have no semantic meaning, which partially contradicts with Solid++'s focus on building a massive decentralized semantic knowledge graph.

## Table of Contents



## Identifying the Endpoint

Not all pods have the same configuration. Before querying a sparql endpoint check to be sure it is enabled, and if so use the endpoint provided. Do not assume it is /sparql.

Request:
```
curl -X GET \
  https://mypod.com/.well-known/pod-configuration \
```

Response:
```
{
  ...
  "rdf_supported": true,
  "rdf_endpoint": "https://mypod.com/rdf",
  ...
}
```

## Data Dump

An rdf data dump can be obtained through sending a GET request to the rdf endpoint. You may change the format of the dump via the `Accept` header. The following formats are available:

* text/turtle
* application/json
* application/n-quads
* application/n-triples
* application/rdf+xml
* application/trig
* application/trix
* application/x-direct-upis
* application/x-turtle
* text/integer
* text/nquads
* text/plain
* text/rdf+n3
* text/table
* text/x-nquads

**Note** Data dumps can only happen if you have been granted access to read everything on the pod.

Request:
```
GET https://mypod.com/rdf
Authorization: Bearer MY_AUTH_TOKEN
Accept: text/turle
```

Response Body:
```
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix dbr: <http://dbpedia.org/resource/> .
@prefix dbo: <http://dbpedia.org/ontology/> .
@prefix dbp: <http://dbpedia.org/property/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<http://dbpedia.org/resource/Alpharetta,_Georgia>
  rdf:type dbo:City ;
  rdf:label "Alpharetta, Georgia"^^xsd:string ;
  dbo:populationTotal 57551 ;
  dbo:isPartOf <http://dbpedia.org/resource/Georgia_(U.S._state)> ;
  dbo:country dbr:United_States .

<http://dbpedia.org/resource/Hendersonville,_North_Carolina>
  rdf:type dbo:City ;
  rdf:label "Hendersonville, North Carolina"^^xsd:string ;
  dbo:populationTotal 13137 ;
  dbo:isPartOf dbr:North_Carolina ;
  dbo:country dbr:United_States .
...
```

## Data Upload

Sending a POST request to the rdf endpoint will add all triples to the pod's graph. You can tell the server the format that is being uploaded in the `Content-Type` header.

**Note** Data uploads can only happen if you have been granted access to add all of the triples that are in your upload.

**Note** Uploading triples this way will create duplicates if an identical triple already exists.

Request:
```
POST https://mypod.com/rdf
Authorization: Bearer MY_AUTH_TOKEN
Content-Type: text/turle
```

Request Body:
```
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix dbr: <http://dbpedia.org/resource/> .
@prefix dbo: <http://dbpedia.org/ontology/> .
@prefix dbp: <http://dbpedia.org/property/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<http://dbpedia.org/resource/Alpharetta,_Georgia>
  rdf:type dbo:City ;
  rdf:label "Alpharetta, Georgia"^^xsd:string ;
  dbo:populationTotal 57551 ;
  dbo:isPartOf <http://dbpedia.org/resource/Georgia_(U.S._state)> ;
  dbo:country dbr:United_States .

<http://dbpedia.org/resource/Hendersonville,_North_Carolina>
  rdf:type dbo:City ;
  rdf:label "Hendersonville, North Carolina"^^xsd:string ;
  dbo:populationTotal 13137 ;
  dbo:isPartOf dbr:North_Carolina ;
  dbo:country dbr:United_States .
...
```

Response Headers:
```
status: 201
```

## Data Delete

Sending a DELETE request to the rdf endpoint will remove all provided triples from the pod's graph. You can tell the server the format that is being provided in the `Content-Type` header.

**Note** Data deletions can only happen if you have been granted access to delete all of the triples that are in your request body.

**Note** Deleting triples this way will remove one triple per instance. If there are multiple identical triples in the pod's graph, and only one is provided in the request body, only one will be deleted.

Request:
```
DELETE https://mypod.com/rdf
Authorization: Bearer MY_AUTH_TOKEN
Content-Type: text/turle
```

Request Body:
```
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix dbr: <http://dbpedia.org/resource/> .
@prefix dbo: <http://dbpedia.org/ontology/> .
@prefix dbp: <http://dbpedia.org/property/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<http://dbpedia.org/resource/Alpharetta,_Georgia>
  rdf:type dbo:City ;
  rdf:label "Alpharetta, Georgia"^^xsd:string ;
  dbo:populationTotal 57551 ;
  dbo:isPartOf <http://dbpedia.org/resource/Georgia_(U.S._state)> ;
  dbo:country dbr:United_States .

<http://dbpedia.org/resource/Hendersonville,_North_Carolina>
  rdf:type dbo:City ;
  rdf:label "Hendersonville, North Carolina"^^xsd:string ;
  dbo:populationTotal 13137 ;
  dbo:isPartOf dbr:North_Carolina ;
  dbo:country dbr:United_States .
...
```

Response Headers:
```
status: 200
```