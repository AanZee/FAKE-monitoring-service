# FAKE Monitoring Service

This repo contains a HELM Chart capable of spinning up a cluster consisting of the following:
- [Fluent Bit](https://fluentbit.io/)
- [APM Server](https://www.elastic.co/products/apm)
- [Kibana](https://www.elastic.co/products/kibana)
- [ElasticSearch](https://www.elastic.co/products/elasticsearch)

This allows us to do the following:
- Collect and parse logs across all our pods and the internal kubernetes logs (Fluent Bit)
- Collect metrics across various application (APM Server) 
- Track users through the the various APIs (APM Server)
- Allow various dahsboards to be build (Kibana)
- Allow reports to be generated (Kibana)
- Allow the features above to interoperate and if needed to be correlated together (Elasticsearch)


## Project structure
The project consists of a [HELM Chart](https://helm.sh/docs/developing_charts/) which described the interaction and configuration of various services. 
To make our service more portable in [Rancher](https://rancher.com/) We add a couple of files described [here]().

| file               | purpose                                                                |
|:-------------------|:-----------------------------------------------------------------------|
| app-read.md        | Decorates the Rancher App page                                         |
| Chart.yaml         | Contains various meta data describing our service                      |
| docker-compose.yml | Contains an example of the service usable by a docker-compose commando |
| README.md          | Contains the description of the service visible in Rancher             |
| requirements.lock  | Contains the locked down versions specified in the file below          |
| requirements.yaml  | Contains Helm Chart this service is dependendant on                    |
| values.yaml        | Contains configuration values                                          |

## Local Development
Due to the complex nature of Kuberenetes not everything can be properly tested locally, to at least attempt at running it locally you can do the following:

- Enable Kubernetes in your docker daemon
- Install K8S Helm for helm file support
  - `brew install kubernetes-helm`
- Initialize Tiller on the local K8S
  - `helm init`
- Add elastic repo for the latest helm files containing elasticsearch and kibana but not apm-server :(
  - `helm repo add elastic https://helm.elastic.co`
- Update dependencies by using in the monitoring servcice folder
  - `helm dependency update`
- Install the service on your local K8S cluster using the name fake
  - `helm install --debug ./monitoring-service --name fake`
- Expose the port for Kibana on your host machine
  - `kubectl port-forward <kibana-docker-id> 5601`
- Delete the service to free up resources
  - `helm del --purge fake `

## Rancher Interaction
To use our fancy new service as an app in your fancy new Rancher setup you have to do the following:
- Add the github repo as a Rancher Catalog (**Do note that itbucket repo's don't seem to work due to some internal rancher issue**)
- Launch the app
- Change the Kibana Ingress to the preferred value

## Future work
This is not production ready due to the following:
- ElasticSearch data is not persisted due to disabled settings
- ElasticSearch is not using a username/password
- Kibana is accessible by everyone
- Resources might be on the low side for a production version
- RDAC and roles are not used but should be
- All data is shipped to ElasticSearch, some should be redacted due to privacy concerns
- Logstash format might be incomplete due to some settings
- Questions.yaml file is missing and could be used to specify Ingress settings before spinning everything up in Rancher
- templates/_helper.tpl file is missing and could be used to guide a user to the running Kibana instance 
- The Other README.md file is lacking in up to date information
