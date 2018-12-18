# Webhooks

Webhooks help you alert external services to changes that have happened on your pods. Currently webhooks work by configuring a [script](localizedCode.md) that trigers at the right moment to make an http request to your service. Ensure that your domain is listed under the "allowedDomains" section in the script's authorization role.