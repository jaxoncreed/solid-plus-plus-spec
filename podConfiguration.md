# Pod Configuration

Each pod might have a slightly different implementation. In order to ensure automated systems can still understand and adapt the these differences, all pods must expose a configuration document at `/.well-known/pod-configuration`. This will expose a document that describes a pod's capabilities.

## Retrieving the Configuration

Request:
```
GET https://mypod.com/.well-known/pod-configuration
```

Response:
```
{
  "sparql_supported": true,
  "sparql_endpoint": "https://mypod.com/sparql",
  "graphql_supported": true,
  "graphql_endpoint": "https://mypod.com/graphql",
  "rdf_supported": true,
  "rdf_endpoint": "https://mypod.com/rdf",
  "supported_rdf_types": [
    "turtle",
    "xml"
  ], 
  "triple_pattern_fragment_supported": true,
  "triple_pattern_fragment_endpoint": "https://mypod.com/tpf"
  "resource_endpoints": ["https://mypod.com/resources/", "https://mypod.com/ontology/"] 
}
```

## Configuration Values

TODO: describe values in depth, but for now it seems self explanatory to someone familiar with semantic web technology.