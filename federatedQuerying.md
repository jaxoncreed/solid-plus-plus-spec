# Federated Querying

The process of querying multiple pods at once and aggregating their answers is called [Federated Querying](https://www.w3.org/TR/sparql11-federated-query/). Essentially the goal is to maintain the illusion to the engineer writing the queries that all data they have access to can be done by making one query to an endpoint. In practice, this is done by sending multiple queries to many endpoints then combining their results in one location.

## Table of Contents

## Query Federation of Known Services

If you know the pods where your data is located, you can use SPARQL's built in `SERVICE` statement. This gives you fine tuned control over the queries and what you are selecting from each service. You can read more about implementing that [here](https://www.w3.org/TR/sparql11-federated-query/).

## Query Federation of Unknown Services

Often you'll want to query all data sources you have access to without knowing their contents. This can simply be done by adding the `federated=true` flag to your url.

```
https://mypod.com/sparql?federated=true&query=SELECT...
https://mypod.com/graphql/GRAPHQL_ID?federated=true&query=...
```

From there it is the responsibility of the pod to query all data sources you have access to and return the results.