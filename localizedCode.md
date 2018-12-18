# Localized Code

Pod owners can consent to have scripts run on their systems 

## Table of Contents

## Script Configuration

Scripts have two kinds of configurations: authorization configurations and trigger configurations. To learn more about authorization configurations, see the script section of the [authorization documentation](authorization.md).

Trigger configurations tell the pod when to tigger the script. I can either be done on an interval like a cron job following the [crontab syntax](https://crontab.guru/), or based on when there was an add or delete to a certain subgraph defined by a CONSTRUCT query. Below is an example configuration:

```
scripts:exampleScript
  a o:Script ;
  rdf:label "A script that triggers when something is added. Or every minute"^^xsd:string ;
  o:addQuery "CONSTRUCT { ?s ?p ?o } WHERE {{ GRAPH ?g { ?s ?p ?o . }}"^^xsd:string ;
  o:cronTrigger "* * * * *"^^xsd:string ;
  o:scriptFile <https://externalScriptFile.com/script.js> .
```

## Writing the script

Scripts are written in JavaScript. When they are run on a pod, you can access various methods for interacting with the pod using the `services` global variable.

TODO: Document the methods in the services variable.