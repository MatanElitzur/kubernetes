# Kubectl

[Images Folder](./images/)
## Links:
https://kubernetes.io/docs/home/
https://github.com/kubernetes/examples
---
## Commands
- kubectl --> Then press enter, will display all commands
---
## Version
- kubectl version
---
## apply
- kubectl apply [resource]
- kubectl apply -f pod.yml --> post the pod defenition to the cluster
- kubectl apply -f file.deployment.yml --> create or apply deployment from file
Kubernetes namespace
- kubectl apply -f .k8s --> applay all files within .k8s folder
---
## cluster-info
- kubectl cluster-info --> view cluster information
---
## create
- kubectl create [resource]
- kubectl create -f file.pod.yml --dry-run --validate=true --> perform trial and validate the yml (validate is the default)
- kubectl create -f file.pod.yml --> Will error if Pod already exists
- kubectl create -f file.pod.yml --save-config --> Store current properties in resources annotation for update
---
## delete
- kubectl delete -f .k8s --> delete a folder that contains alll k8s files
- kubectl delete pod [name-of-pod] --> Example kubectl delete pod hello-pod
- kubectl delete -f multi-pod.yml --> the delete what is define in the yml
- kubectl delete svc [name-of-svc] --> delete a specific K8s service in your namespace
- kubectl delete deployment [name-of-deployment] --> Delete the deployments that manages the Pod
- kubectl delete namespace [namespace-to-delete]
---
## describe
- kubectl describe deployment [name-of-deployment] --> show info on deployment
  - Example: kubectl describe deployment portal-cf-spc-service --kubeconfig="./config/template/kubeconfig_with_token.yaml" -n devops
- kubectl describe pod [name-of-pod] --> show info on pod
- kubectl --kubeconfig=kubeconfigViaCockpit.yaml describe servicebinding portal-feature-flags-binding  -n [namespace]
  - Example: kubectl --kubeconfig=kubeconfigViaCockpit.yaml describe servicebinding portal-feature-flags-binding  -n lep
- kubectl describe svc ps-nodeport  --> show info on service name ps-nodeport
    - get the Type, like NodePort
    - get the IP, like 10.128.232.224
    - get the IPs, like 10.128.232.224
    - get the Port, like <unset> 80/TCP
    - get the TargetPort, like 8080/TCP
    - get the Node Port, like <unset> 31111/TCP
    - get the Endpoints, like 10.2.1.2:8080
---
## edit
- kubectl edit -f nginx.pod.yml
---
## exec
- kubectl exec [pod-name] -it sh  --> go into a pod
---
## get
- kubectl get all --> get all the resources
- kubectl get configmaps
  - Example: kubectl get configmaps --kubeconfig="./config/template/kubeconfig_with_token.yaml" -n devops
- kubectl get events --sort-by='.lastTimestamp'
  - Example: kubectl get events --sort-by='.lastTimestamp' --kubeconfig='./config/template/kubeconfig_with_token.yaml' -n devops
  - Example: kubectl get events --kubeconfig='./config/template/kubeconfig_with_token.yaml' -n lep
- kubectl get deploy --> get info about the deployment
- kubectl get deployment --show-labels --> list all deployments and their labels
  - Example: kubectl get deployment --kubeconfig='./config/template/kubeconfig_with_token.yaml' -n devops
- kubectl get deployment -l app=nginx --> list all deployments with a specific label and it value
- kubectl get deployments
- kubectl get ep --> get the end points
- kubectl get pod [name-of-pod] -o yaml --> get the info as ymal output
- kubectl get pod [name-of-pod] -o json --> get the info as json output
- kubectl get pods --> Show all the pods
  - Example: kubectl get pods --kubeconfig='./config/template/kubeconfig_with_token.yaml' -n devops
- kubectl get pods --watch
- kubectl get pods -o wide
- kubectl get pods --show-labels
- kubectl get nodes --> get info about the running nodes
  - Example: kubectl get nodes --kubeconfig='./config/template/kubeconfig_with_token.yaml' -n devops
- kubectl get rs --> get the replica sets
- kubectl get services --> show all the services
- kubectl get svc --> display information about the services in your
- kubectl get all --> display all info
---
## rollout
- kubectl rollout history deploy web-deploy --> web-deploy is the name of the deployment, we see the revisions
- kubectl rollout undo deploy web-deploy --ro-revision 1 --> web-deploy is the name of the deployment
---
## Run
- kubectl run [container-name]
- kubectl run --image=[image-name]
    - Example: kubectl run my-nginx --image=nginx:alpine
---
## port-forward
- kubectl port-forward [pod] [ports] --> open the pod ports to external world, browser
- kubectl port-forward [name-of-pod] 8080:80 --> Enable Pod container to be called externally via 8080 port and internal 80 port.
---
## Replicas
- kubectl scale deployment [deployment-name] --replicas=5 --> scale the deployment Pods to 5
- kubectl scale -f file.deployment.yml --replicas=5 --> scale the deployment Pods to 5

---
## Configmap
[configMaps Folder](./images/configMaps/)
- kubectl create configmap [cm-name] --from-file=[path-to-file] --> From config file
- kubectl create configmap [cm-name] --from-env-file=[path-to-file] --> From env file
- kubectl create configmap [cm-name] 
  --from-literal=apiUrl=https://my-api
  --from-literal=otherKey=otherValue --> from individual data valus
- kubectl create -f file.configmap.yml --> Create from a ConfigMap manifest (like: kind:ConfigMap)
- kubectl get cm [cm-name] -o yaml --> Get a configMap and view its contents as yaml display
- kubectl get cm --> display all ConfigMaps
___
## Secrets
[secrets Folder](./images/secrets/)
- kubectl create secret generic my-secret 
  --from-literal=pwd=my-password
  --> Create a secret and store securely in K8s
- kubectl create secret generic my-secret
  --from-file=ssh=privatekey=~/.ssh/id_rsa
  --from-file=ssh=publickey=~/.ssh/id_rsa.pub
  --> Create a secret from a file
- kubectl create secret tls tls-secret --cert=path/to/tls.cert   --key=path/to/tls.key
--> Create a secret from a key pair
- kubectl get secrets
  - Example: kubectl get secrets --kubeconfig="./config/template/kubeconfig_with_token.yaml" -n devops
- kubectl get --kubectl=[PATH] get secret [SECRET_NAME] -n [NAMESPACE] - yaml
  - Example: kubectl --kubeconfig=kubeconfigViaCockpit.yaml get secret portal-feature-flags-binding -n lep -o yaml 
- kubectl get secrets db-passwords -o yaml
___
## Troubleshooting Techniques
#### logs
- kubectl logs [pod-name] --> View the logs got a Pod's container
  - Example: kubectl logs portal-cf-spc-service-7474945fb4-djqvv --kubeconfig='./config/template/kubeconfig_with_token.yaml' -n devops
- kubectl logs [pod-name] -c [container-name] --> View the logs for a specific container within a Pod
- kubectl logs -p [pod-name] --> View the logs for a previously running Pod
- kubectl logs -f [pod-name] --> Stream a Pod's logs
#### describe
- kubectl describe pod [pod-name] --> get details about a pod
  - Example: kubectl describe pod portal-cf-spc-service-7474945fb4-djqvv --kubeconfig='./config/template/kubeconfig_with_token.yaml' -n devops
- kubectl get pod [pod-name] -o ymal --> get details about a pod
- kubectl get deployment [deployment-name] -o ymal --> get details about a pod
- kubectl exec [pod-name] -it sh --> Shell into a Pod container

#### SAP
- kubectl --kubeconfig=kubeconfig.yaml auth can-i list [resource] -n [namespace_name]
  - The [resource] can be pods, secrets, configmaps, etc..
  - Example: kubectl --kubeconfig=kubeconfig.yaml auth can-i list pods -n lep
    - if the answer is yes, now you can invoke     
     for example: kubectl --kubeconfig=kubeconfig.yaml get pods -n poc
- kubectl --kubeconfig=kubeconfig.yaml config current-context --> Display current cluster name
- kubectl --kubeconfig=kubeconfig.yaml config use-context [cluster_name] --> switch to a different clustr
   - Example: kubectl --kubeconfig=kubeconfig.yaml config use-context garden-kyma-stage--c-3a4b28b-external-2
- kubectl --kubeconfig=kubeconfig.yaml config get-contexts --> Display all clusters for current kubeconfig.yaml
