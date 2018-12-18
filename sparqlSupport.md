# SPARQL Support

Pods can optionally support [SPARQL](https://www.w3.org/TR/rdf-sparql-query/), a w3c standard for querying linked data. In this document we outline the interface used to query a pod using SPARQL.

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
  "sparql_supported": true,
  "sparql_endpoint": "https://mypod.com/sparql",
  ...
}
```

## Sending a Query

Sending a query can either be done with a GET or POST request. Update queries can only be performmed with POST Requests. Represented with SPO examples below:

Request:
```
GET https://mypod.com/sparql/?query=SELECT ?s ?p ?o WHERE { ?s ?p ?o }
Authorization: Bearer MY_AUTH_TOKEN
```

Request:
```
POST https://mypod.com/sparql/
Authorization: Bearer MY_AUTH_TOKEN
```

Request Body:
```
SELECT ?s ?p ?o WHERE { ?s ?p ?o }
```
