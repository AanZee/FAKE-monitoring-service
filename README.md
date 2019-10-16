### Helm files 

- Enable Kubernetes in your docker deamon
- Install K8S Helm for helm file support
  - `brew install kubernetes-helm`
- Initialize Tiller 
  - `helm init`
- Add elastic repo for the latest helm files containing elasticsearch and kibana but not apm-server :(
  - `helm repo add elastic https://helm.elastic.co`
- Update dependencies by using in the monitoring servcice folder
  - helm dependency update

 

helm dependency update
