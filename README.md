# sko-2022
Nick & Aaron here...helping others learn about a couple of our favorite technologies...Salt and Kong!

<img src=https://user-images.githubusercontent.com/223486/157987198-8ebf4b8a-1b02-4b5c-b5c9-5a8d6bae290f.png width="600px">

# Prerequisites for this lab
1. [Docker](https://docs.docker.com/get-docker/)
2. Only if you're on Linux: [Docker Compose](https://docs.docker.com/compose/install/)

# Setting up Salt's Inline Solution
As you can see above, Salt's Plugin mirrors traffic being proxied by Kong.

To accomplish this, the plugin takes the following three required parameters (review [the doc](https://saltsecurity.atlassian.net/wiki/spaces/PROD/pages/461930619/Kong+1.3+Traffic+Collector+Deployment+Guide) for the others):

| Parameter      | Description |
| ----------- | ----------- |
| salt_url      | \[hybrid_url \| https://traffic-receiver-http-ns-a.dnssf.com]   |
| salt_token   | <YOUR_TRAFFIC_COLLECTION_INSTALLATION_TOKEN>        |
| salt_uuid    | https://httpbin.org/uuid |   

### Let's assume you've cloned this repo and are in the sko-2022 directory...

1. `docker-compose up -d`
This spins up a pre-built container of Kong with [Salt's Kong Policy](https://admin-panel-prod-a.secured-api.com/#/downloads) loaded in

Because we haven't yet started proxying anything, if you run `curl localhost` or `curl -k https://localhost` you'll see a 404 `{"message":"no Route matched with those values"}`

2. edit the httpbin.yaml file, insert your TOKEN, save and exit.

3. `curl -X POST localhost:8001/config -F config=@httpbin.yaml`
This loads the file you just configured which in addition to the plugin definition, contains a definition for a service and its route.

4. `curl localhost/anything` to see https://httpbin.org/anything reverse-proxied and the traffic mirrored to Datadog. This is Kong proxying, on port 80, the httpbin.org service.
5. `curl localhost:8001/plugins` to review what was configured for the Salt Plugin

# Next Steps
- Setup a Hybrid and configure the httpbin.yaml file to talk to it instead
- Review [the documentation](https://saltsecurity.atlassian.net/wiki/spaces/PROD/pages/461930619/Kong+1.3+Traffic+Collector+Deployment+Guide)
- Try configuring the Salt Plugin to operate on all services and not just one
- Try mirroring traffic for one service but not another
