# Helm - packaging and versioning an app
#Packaging Applications with Helm for Kubernetes
by Philippe Collignon

[Helm Images Folder](./images/Helm)

## Links
https://github.com/phcollignon/helm3

## Helm charts repositories
- https://artifacthub.io
  #### install mongodb Example
  - helm repo add bitnami https:..charts/bitnami.com/bitnami
  - helm install my-relase bitnami/mongodb
___
## install minikube
Link: http:/minikube.sigs.k8s.io
#### install minikube (This is a K8s cluster running on docker)
- curl -LO https://storage.googleapis.com/minikube/releases/latest/minkube-linux-amd64
- sudo install minkube-linux-amd64 /usr/local/bin/minikube
- minikube version
- minikube start
- minikube status --> Check that cluster is running
- minikube addons enable ingress --> we need it to access the demo
- minikube ip --> get the cluster ip
- sudo vi /etc/hosts --> adding the ip
- minikube tunnel (for MAC os)
___
#### install kubectl
- curl -o kubectl https://storage.googleapis.com/kubernetes-release/release/v1.23.3/bin/linux/amd64/kubectl
- chmod u+x kubectl
- sudo mv kubectl /usr/local/bin
- kubectl version --short
- kubectl config view
---
### install helm 
Link: https://helm.sh/
- curl -LO https://get.helm.sh/helm-v3.8.2-linux-amd64.tar.gz
- tar -zxvf helm-v3.8.2-linux-amd64.tar.gz
- sudo mv linux-amd64/helm /usr/local/bin/helm
- helm version --short
- helm repo add stable https:// charts.helm.sh/stable --> install offical helm charts repository (This https:// charts.helm.sh/stable is not maintain any more so you need to work with thired party repositories)
- helm repo add myrepo http://myserver.org/charts
- helm repo remove myrepo
- helm install demo-mysql stable/mysql --> install mysql for the demo from the offical hem charts repository. After that we can invoke kubectl get all | grep mysql and see the demo- mysql
___
### uninstall helm (demo-mysql)
- helm uninstall demo-mysql

## helm commands
- helm install [release] [chart] --> Install a Release
  - Example: helm install demo-gustbook gustbook
- helm upgrade [release] [chart] --> Upgrade a Release revision (Like for a new app version)
- helm upgrade -i [release] [chart] --debug --> Upgrade the Helm release [release] using chart at [chart], or install it if it doesn’t exist
  - You can use also --install insted of -i
- helm rollback [release] [chart] --> Rollback to a Release revision
  - Example rollback demo-gustbook 1 --> rollback to the release demo-gustbook and to revision 1
- helm history [release] --kubeconfig="[PATH]" -n [namespace name]--> Print Release history
- helm status [release] -n [namespace] --> Display Release status
  - Example: helm status --kubeconfig="./config/template/kubeconfig_with_token.yaml" portal-cf-spc-service -n lep
- helm get all [release] --> Show details of a release
- helm uninstall [release] --kubeconfig="[PATH]" -n [your-namespace] --> Uninstall a release
  - Example: helm uninstall portal-cf-spc-service --kubeconfig="./config/template/kubeconfig_with_token.yaml" -n devops
- helm uninstall --keep-history [release] --> Uninstall a release but keep the history in the helm tree
- helm list --> List Releases
  - Example: helm list --short --kubeconfig="./config/template/kubeconfig_with_token.yaml" -n devops
- helm get manifest [release]
  - Example: helm get manifest demo-gustbook | less
  - Example: helm get manifest portal-cf-spc-service --kubeconfig="./config/template/kubeconfig_with_token.yaml" -n devops | less
- helm repo list --> list all the repositories in your repo
- helm inspect [all or readme or chart or values] [chart_name]
- helm show values chart_name
- helm fetch chart_name --> download a chart without dependencies
- helm dependency update chart_name --> download stable dependecies into the chart subfolders

#### Helm Template Test
| static | Dynamic |
|----------|----------|
| helm template [chart]   | helm install [release][chart] --dry-run --debug   |
| works without K8s cluster   | real helm install but without commit   |
| static release-name | can generate a release-name|
- helm template [chart] --> Check the chart, it display the maifest build by the engine
 - Example: helm package portal-cf-spc-service
- helm install [release][chart] --dry-run --debug --> check the cart with more display computed values
helm install [release][chart] --dry-run --debug 2>&1 |less

### Functions and Pipelines
- Go text/template packages http://golang.org/pkg/text/template 
- Sprig functions project http://masterminds.github.io/sprig
- Helm project http:/helm.sh

### Packaging a Chart
- helm package [cart_name]
- helm repo index . --> create the index.html file in the folder containg the helm charts
- helm package --sign --> sign a chart with a valid pgp key
- helm verify chart.tgz
- helm install --verify
- Thired party repository 
   - curl -LO https://s3.amazonaws.com/chartmuseum/release/latest/bin/linux/amd64/chartmuseum
   - chmod u+x chartmuseum --> make it executable
   - sudo mv chartmuseum /usr/local/bin --> Move it to the executable folder
   - mkdir ~/helm/repo --> storage location for the repository in the demo
   - chartmuseum --storage="local" --storage-local-rootdir=/home/me/helm/repo --> to start chartmuseum and it will listen in port 8080
   - cp *.tgz /home/me/helm/repo --> copy the tgz charts to the local storage
   - curl http://localhost:8080/api/charts | jq . --> create a request to get the list of charts
   - helm repo add chartmuseum http://localhost:8080
   - helm search repo chartmuseum/ --> to display which charts are available in the chart repository
- helm dependency update [chart_name] --> download the sub dependencies
- helm dependency list [chart_name] --> to check which carts are a vailables
- helm dependency build [chart_name] --> to stick with the same subcharts versions as mentioed in Chart.lock file so we get the same versions
