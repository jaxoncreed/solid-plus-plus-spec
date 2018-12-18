# Access Control

## Table of Contents

## Key Concepts

Access control over a pod's graph is defined by roles. There can be any number of roles on a specific pod. There are two kinds of roles: pod roles, and client roles:

### Pod Roles

Pod roles define what requests bearing a token representing a specific pod can do. They have various attributes:

* label: A short name for the role
* description (optional): A description of the role
* readQuery (optional): A SPARQL construct query defining the subgraph from which members of this role are allowed to read. Leaving this blank will restrict read access to all triples.
* insertQuery (optional): A SPARQL construct query defining the subgraph to which members of this role are allowed to add (If a added triple is within this query, it is allowed to be added). Leaving this blank will restrict the ability to add any triples.
* deleteQuery (optional): A SPARQL construct query defining the subgraph from which members of this role are allowed to delete (If a triple is within this query, it is allowed to be deleted). Leaving this blank will restrict the ability to delete any triples.
* whitelistedRoleMember*: Each role can have a variable number of members made up of pod urls or a reference to `https://o.team/ontology/AllPods` (Denoting all pods are members). These denote the pods who are a part of this role.
* blacklistedRoleMember*: The blacklist overrides the whitelist. If a pod is a member of both the blacklist and the whitelist, it will not count as a member of this role (This is especially useful for roles where `AllPods` is in the whitelist)

Example:
```
roles:Owner
  a o:Role ;
  a o:PodRole ;
  rdf:label "Owner Role"^^xsd:string ;
  rdf:description "A role that grants access to do all triple operations"^^xsd:string ;
  o:readQuery "CONSTRUCT { ?s ?p ?o } WHERE {{ GRAPH ?g { ?s ?p ?o . }}"^^xsd:string ;
  o:insertQuery "CONSTRUCT { ?s ?p ?o } WHERE {{ GRAPH ?g { ?s ?p ?o . }}"^^xsd:string ;
  o:deleteQuery "CONSTRUCT { ?s ?p ?o } WHERE {{ GRAPH ?g { ?s ?p ?o . }}"^^xsd:string ;
  o:whitelistedRoleMember pod: .
```

If a pod is a member of multiple roles, as long as a triple operation is granted by at least one role the operation will be granted.

### Client Roles

Requests that originate from [clients secured by an Solid++ extension](securingClient.md) must reference both the set of Pod Roles and the set of Client Roles. If either does not contain a role that grants the triple operation, the operation will be blocked. Client roles are evaluated on the client by the Solid++ extension. If its operation is granted, it will send a request to the pod to be validated and to conditionally carry out its requested operation.

Client roles have the following attributes:

* label: A short name for the role
* description (optional): A description of the role
* readQuery (optional): A SPARQL construct query defining the subgraph from which requests originating from the domain of this role are allowed to read. Leaving this blank will restrict read access to all triples.
* insertQuery (optional): A SPARQL construct query defining the subgraph to which requests originating from the domain of this role are allowed to add (If a added triple is within this query, it is allowed to be added). Leaving this blank will restrict the ability to add any triples.
* deleteQuery (optional): A SPARQL construct query defining the subgraph from which requests originating from the domain of this role are allowed to delete (If a triple is within this query, it is allowed to be deleted). Leaving this blank will restrict the ability to delete any triples.
* **allowedDomains: A list of domains to which this client is allowed to make requests**
* whitelistedRoleClients*: Each role can have a variable number of members made up of **client urls**, or the word "all." These denote the clients who are a part of this role.
* blacklistedRoleClients*: The blacklist overrides the whitelist. If a client is a member of both the blacklist and the whitelist, it will not count as a member of this role (This is especially useful for roles where `AllDomains` is in the whitelist)

Example:
```
roles:Default_Domain
  a o:Role ;
  a o:ClientRole ;
  rdf:label "Default Domain Role"^^xsd:string ;
  o:readQuery "CONSTRUCT { ?s ?p ?o } WHERE {{ GRAPH ?g { ?s ?p ?o . }}"^^xsd:string ;
  o:allowedDomains "mypod.com", "cdn.o.team" ;
  o:whitelistedRoleDomain o:AllDomains .
```

### Script Roles

[Scripts](localizedCode.md) also have roles with the following attributes:

* label: A short name for the role
* description (optional): A description of the role
* readQuery (optional): A SPARQL construct query defining the subgraph from which requests originating from the domain of this role are allowed to read. Leaving this blank will restrict read access to all triples.
* insertQuery (optional): A SPARQL construct query defining the subgraph to which requests originating from the domain of this role are allowed to add (If a added triple is within this query, it is allowed to be added). Leaving this blank will restrict the ability to add any triples.
* deleteQuery (optional): A SPARQL construct query defining the subgraph from which requests originating from the domain of this role are allowed to delete (If a triple is within this query, it is allowed to be deleted). Leaving this blank will restrict the ability to delete any triples.
* allowedDomains: A list of domains to which this script is allowed to make requests.
* whitelistedRoleScripts*: Each role can have a variable number of members made up of **script ids**, or the word "all." These denote the clients who are a part of this role.
* blacklistedRoleScripts*: The blacklist overrides the whitelist. If a script is a member of both the blacklist and the whitelist, it will not count as a member of this role (This is especially useful for roles where `AllScripts` is in the whitelist)

Example:
```
roles:Default_Scripts
  a o:Role ;
  a o:ScriptRole ;
  rdf:label "Default Script Role"^^xsd:string ;
  o:readQuery "CONSTRUCT { ?s ?p ?o } WHERE {{ GRAPH ?g { ?s ?p ?o . }}"^^xsd:string ;
  o:whitelistedRoleScripts o:AllScripts 
  o:allowedDomains: "externalside.com" .
```

## Default Roles (Optional)

While pods could start with any set of default roles, standard pods should start out with these default roles ([The entire default graph can be seen here](assets/defaultGraph.ttl)).

* Owner: Grants the ability for members to do anything. Begins with only a reference to itself as a member.
* Read_All: Allows pods to read all data
* Write_All: Allows pods to add and delete triples everywhere except for roles.
* Read_Roles_if_Member: A role that allows all members of each role to see the role of which they are a member.
* Default_Domain: Allows access to read everything on this pod and make requests to this pod. **If a website wants to make requests to any other sites outside of this pod, it is recommended to blacklist it from this role.**

## Creating a Role

### Via RDF

The [RDF interface](rdfSupport.md) can be used to create a role, given your token represents a pod with the correct access permission to create a role:

Request:
```
POST https://mypod.com/rdf
Authorization: Bearer MY_AUTH_TOKEN
Content-Type: application/trig
```

Request Body:
```
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix o: <https://o.team/ontology/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix roles: <https://mypod.com/resources/roles/> .
roles:Sample_Role
    a o:Role ;
    a o:PodRole ;
    rdf:label "Sample Role"^^xsd:string ;
    rdf:description "A role that serves as a sample"^^xsd:string ;
    o:readQuery "CONSTRUCT { ?s ?p ?o } WHERE {{ GRAPH ?g { ?s ?p ?o . }}"^^xsd:string .
```

Response Headers:
```
Status: 201
```

### Via GraphQL

The [default graph](assets/defaultGraph.md) includes configurations for a [GraphQL interface](graphqlSupport.md) that supports creating roles:

Request:
```
POST https://mypod.com/graphql/roles
Authorization: Bearer MY_AUTH_TOKEN
```

Request Body:
```
{
  "query" {
    "createRole" {
      "roleType": "PodRole",
      "label": "Sample Role",
      "description": "A role that serves as a sample",
      "readQuery: "CONSTRUCT { ?s ?p ?o } WHERE {{ GRAPH ?g { ?s ?p ?o . }}"
    }
  }
}
```

Response Headers:
```
Status: 201
```

### Writing a CONSTRUCT Query

CONSTRUCT queries are queries that return a graph. When you define a construct query on a roll you are saying that you grant a certain permission to the hypothetical graph that would get returned by that query. You can [learn more about writing CONSTRUCT queries here](https://www.w3.org/TR/rdf-sparql-query/#construct).

In additoin to the query itself, pods provide a few useful variables that can be plugged into the query at runtime. Surrounding a variable in double brackets ("[[ ... ]]") will allow its value to be inserted at runtime.

For Example:
```
CONSTRUCT { ?s ?p ?o } WHERE { GRAPH ?g { ?s <https://o.team/ontology/roleMember> [[requester]] . ?s ?p ?o } }
```
Will translate to the following given a request came from `requestingpod.com`.

```
CONSTRUCT { ?s ?p ?o } WHERE { GRAPH ?g { ?s <https://o.team/ontology/roleMember> <https://requestingpod.com> . ?s ?p ?o } }
```

There are a few variables you can insert:

* `[[requester]]` The domain of the pod that sent the request.
* `[[me]]` The domain of this pod.

## Checking the Roles to which you have Access

Most pods will allow you to check which roles you have access to. To do this you can send a regular search query to find the pods.

### Via SPARQL

Request:
```
POST https://mypod.com/sparql
Authorization: Bearer MY_AUTH_TOKEN
Accept: application/trig
Content-Type: application/sparql
```

Request Body:
```
CONSTRUCT { ?s ?p ?o } WHERE {{ GRAPH <https://mypod.com/resources/roles> { ?s ?p ?o . }}
```

Response Body:
```
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix o: <https://o.team/ontology/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix n0: <https://mypod.com/resources/> .
@prefix roles: <https://mypod.com/resources/roles/> .

n0:roles {
  roles:Read_Roles_if_Member
    a o:Role ;
    a o:PodRole ;
    rdf:label "Read Roles if Member Role"^^xsd:string ;
    rdf:description "A role that allows members to read information about roles as long as they are a member"^^xsd:string ;
    o:readQuery "CONSTRUCT { ?s ?p ?o } WHERE { GRAPH <https://mypod.com/resources/roles> { ?s a <https://o.team/ontology/Role> . ?s <https://o.team/ontology/roleMember> [[requester]] . ?s ?p ?o } }"^^xsd:string ;
    o:whitelistedRoleMember o:AllPods .
}
```

### Via GraphQL

Request:
```
POST https://mypod.com/graphql/roles
Authorization: Bearer MY_AUTH_TOKEN
```

Request Body:
```
{
  "query" {
    roles {
      roleType
      label
      description
      readQuery
      insertQuery
      deleteQuery
      allowedDomains
      whitelistedRoleMembers
      blacklistedRoleMembers
      whitelistedRoleClients
      blacklistedRoleClients
    }
  }
}
```

Response Body:
```json
{
  data: [
    {
      roleType: "PodRole",
      label: "Read Roles if Member Role",
      description: "A role that allows members to read information about roles as long as they are a member",
      readQuery: "CONSTRUCT { ?s ?p ?o } WHERE { GRAPH <https://mypod.com/resources/roles> { ?s a <https://o.team/ontology/Role> . ?s <https://o.team/ontology/roleMember> [[requester]] . ?s ?p ?o } }",
      whitelistedRoleMembers: [ "AllPods" ]
    }
  ]
}
```

## Requesting Access to a Role

You might have a pod and want to request access to data on a pod that you don't own. To request access, you create a role on your pod with a `o:requestsApprovalFrom` edge pointing to the foreign pod. Then grant the foreign pod access to the requested role. A request will be made to the foreign pod alerting it that something has been shared with it, and the user will be alerted to approve the access request.
