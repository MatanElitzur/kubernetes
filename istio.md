# Istio - is a service mesh
### Links
- course link https://app.pluralsight.com/library/courses/kubernetes-istio-managing-apps/table-of-contents
- https://github.com/istio
- https://github.com/istio/istio.io

### What is a service mesh
service mesh - It's the communication layer between software components, made into its own thing.
- a component is register to a service mesh and it create a network proxy as a side car in the same pod (middleman) so in the service mesh we can declare the address, timeout, retry, encryption so the communication to other componenets are via Istio, So Istio is controling the network traffic, It can help with digging a performance issue and debug failures.
- You(client) --> Proxy Server --> Github.com (This is a network proxy)
why to use a network proxy Security, Filtering / Control, Caching Logging/Monitoring, Bypassing restrictions so we can declare some rules.
- Can configure the external traffic, rate limit, autorization, to the application entry point by the service mesh
- Observability (Istio is using a plugin module for this), and those are the telemetry types, those can send their data to a different backends and Istio provide the integration for all the indestry software like ***Prometheus***, ***Zipkin***, ***Elasticsearch**
  - Metric - the count the number of requests and the duration taken for a responses
  - Distributed Traces - The entire span of a request
  - Access Logs - Low level logs that are coming from the proxies

| Feature                | Purpose                                       |
| ---------------------- | --------------------------------------------- |
| **Traffic management** | Route requests between services intelligently |
| **Observability**      | Collect metrics, logs, and traces             |
| **Security**           | Encrypt service-to-service traffic (mTLS)     |
| **Resilience**         | Retries, circuit breaking, fault injection    |

### Deploying Istio
- helm upgrade --install istio-base istio/base
- helm upgrade --install istiod istio/istiod
- helm upgrade --install istio-ingress istio/gateway

### istioctl
- download istioctl
- istioctl version
- istioctl profile list
- istioctl profile dump > demo1/temp/istio-default-profile.yaml

## Commands
- kubectl label namespace bookinfo istio-injection=enabled
- kubctl apply -f bookinfo.yaml
- kubctl apply -f bookinfo-gateway.yaml

## Components
 - kind: VirtualService - that we use for routing insted of a K8's service
 - kind: DestinationRule - specify the name of the host and the subsets, The subsets is for the selection of the Pods which are register as a K8's service, K8's find their pods by labels


## Commands
- kubectl get po,gateway,virtualservice 