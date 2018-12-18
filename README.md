# Solid++ Standards

This document outlines the Solid++ standards, an ecosysten designed to link all human knowledge into one, decentralized, searchable database while maintaining data privacy for its users.

## Table of Contents

* [Links to Detailed Explanations](#links-to-detailed-explanations)
* [Philosophy](#philosophy)
* [Pods](#pods)
* [Authorizing Access to a Pod](#authorizing-access-to-a-pod)
* [Manipulating Data](#manipulating-data)
* [Granting and Requesting Access to Data](#granting-and-requesting-access-to-data)
* [Querying Everything](#querying-everything)
* [Running Localized Code](#running-localized-code)
* [Securing the Client](#securing-the-client)
* [Webhooks](#webhooks)

## Links to Detailed Explanations

* [Pod Configuration](podConfiguration.md)
* [Authorization](authorization.md)
* Manipulating Data
  * [graphql](graphqlSupport.md)
  * [sparql](sparqlSupport.md)
  * [rdf](rdfSupport.md)
  * [triple pattern fragments](tpfSupport.md)
* [Access Control](accessControl.md)
* [Federated Querying](federatedQuerying.md)
* [Localized Code](localizedCode.md)
* [Securing the Client](securingClient.md)
* [Webhooks](webhooks.md)

## Philosophy

The way data is stuctured online is an embarrassment. Not only have [privacy concerns been a topic of hot debate recently](https://jonathanmayer.org/publications/trackingsurvey12.pdf), but the siloed nature of data has limited its flexibility.

In the pre-internet era, files saved on a computer were under the control of the user and accessible by multiple programs. You could take a picture with one app and edit it with another even if the apps didn't know eachother existed. It would make sense to assume that in an era where many systems can be connected this kind of data freedom would be intensified, however data is increasingly bound to the app.

This paridigm shift is detrimental to both the developers and users. Because data is increasingly centralized a developer needs to build an entire ecosystem (auth system, back end, database, ui) to provide a service to the user. Piggybacking off of current ecosystems is possible via APIs, but [only at the whim of the ecosystem owner](https://thenextweb.com/dd/2015/02/12/linkedin-takes-aim-developers-plans-lock-apis/) and not the user who initially generated the data.

Today the most arduous part of a data scientist's job is [finding and formatting data](https://visit.figure-eight.com/rs/416-ZBE-142/images/CrowdFlower_DataScienceReport.pdf?mkt_tok=eyJpIjoiTTJFek1UaGxNekl5TmpJeCIsInQiOiJPMXpWVnREbHJGR1RHN1BUVk9Zdm5xTEEyNXRpdDRONmZ4XC9oaFwvZGpFNnB0Z0hIWERGS1NqN0huRjB5QUE1UUJVbVZxNFhaeVBSUXJzMTNYdmRJR), and today data comes in many formats from many sources all with slightly different ways to gain access. This pointless overhead is severely limiting the potential of artificial intelligence. Given a system where a data scientist can search all of human knowledge she to which she has access, the power of data can truly be unleased.

Solid++ seeks to rectify these problems by creating an ecosystem that makes it easy to standardize data in one large, decentralized knowledge graph that still respects the privacy of the data's origin.

Solid++ is heavily inspired by [SOLID](https://solid.mit.edu) but with a focus on single-endpoint querying and inherant data privacy.

## Pods

Full Article: [pod configuration](podConfiguration.md)

A user stores all of her data in a pod. Pods are servers that follow the standards outlined in this spec. A user can either deploy host her own pod or use a pod provider, a second party that the user trusts as a worthy vangard for her data.

Currently there is a pod provider deployed by O located at [pod.o.team](https://pod.o.team), however any server that follows these standards can be connected to the Solid++ ecosystem.

## Authorizing access to a Pod

Full Article: [authorization](authorization.md)

Authorizing with a pod results in obtaining a [JSON Web Token](https://jwt.io/) that indicates the requesting party is the owner of the pod. In order to query a pod, a service or user must retrieve a valid token via 2 different ways. See the [authorization](authorization.md) documentation to see more.

## Manipulating Data

There are multiple ways to manipulate data on a pod. Not all pods will support every method. See [pod configuration](podConfiguration.md) for more details.

See the following for the various methods of manipulating data:
* [graphql](graphqlSupport.md): A popular interface that should feel natural to developers familiar with JSON.
* [sparql](sparqlSupport.md): A powerful query language and w3c standard for the semantic web.
* [rdf](rdfSupport.md): A dump of all edges in a user's data graph that will require client side querying
* [triple pattern fragments](tpfSupport.md): A selected dump that allows processing power for queries to be shared between a client and server.


## Granting and Requesting Access to Data

Full Article: [access control](accessControl.md)

Solid++ defines a powerful access control system that can allow users to customize exactly who has access to what data on their pod. When a 3rd party makes a request for data the request will be returned as if only the data to which the 3rd party has access exists.

Access control can either be set by the pod owner, or a message can be sent to ask the pod owner to add a new role to her access control.

## Querying Everything

Full Article: [federated querying](federatedQuerying.md)

Upon querying a server, if federated querying is enabled, the pod will attempt to make a request to all pods it is aware of. Results for federated queries are streaming. Eventually, given enough time, all results will be indicative of all knowledge in the Solid++ ecosystem to which you are granted access.

## Running Localized Code

Full Article: [localized code](localizedCode.md)

As a pod owner, you can allow programs to run on your pod. You can be confident that these programs will not send data outside your pod without your consent. These programs can help you keep your data organized, or perform much needed processing for more complex analytics.

## Securing the Client

Full Article: [securing client](securingClient.md)

To ensure that malicious UIs can't steal user data, there are various ways to secure clients like Web Browsers, Android Phones, and iPhones.

## Webhooks

Full Article: [webhooks](webhooks.md)

Pods can support webhooks to alert your service if some change has been made to the graph.