# Istio

##  About Istio

What is a service mesh? A service mesh is a network of microservices that makeup applications and the interactions between them. 
Requirements for a service mesh are;

  Discovery
  
  Load balancing
  
  Failure recovery
  
  Metrics
  
  Monitoring
  
  A/B testing
  
  Canary releases
  
  Rate limiting
  
  Access control
  
  End-to-end authentication

Istio makes it easy to create a network of deployed services with load balancing, service-to-service authentication, monitoring, etc. 
Istio supports services by deploying a special sidecar proxy throughout the environment that intercepts all network communications 
between micro-services, then configure and manage Istio using control plane functionality which includes -

Automatic load balancing for HTTP, gRPC, WebSocket and TCP traffic

Fine-grained control of traffic behaviour with rich routing rules, retires, failovers and fault injection

A pluggable policy layer and configuration API 

Automatic metrics, logs and traces


# ISTIO Architecture

![alt text](https://cdn-images-1.medium.com/max/1000/1*Il6GOrXIkfzrkRKtOC-bSA.png)

An Istio service mesh is consist of two parts as, data plane and control plane

Data plane — 

It is composed of a set of intelligent proxies named Envoy which is deployed as a sidecar. 
These proxies mediate and control all the network communication between micro-services along with Mixer (a general-purpose and telemetry hub)

Control plane — 

Manages and configures the proxies to route traffic. 
Plus it configures Mixers to enforce policies and collect telemetry.

## Envoy

Envoy is a high-performance proxy developed in C++ to mediate all inbound and outbound traffic for all services in the service mesh

Some built-in features:

Dynamic service discovery
Load balancing
TLS termination
HTTP/2 and gRPC proxies
Circuit breakers
Health checks
Staged rollouts with %-based traffic split
Fault injection
Rich metrics

Envoy is deployed as a sidecar to the relevant service in the same Kubernetes pod. 


##  Mixer

The mixer is a platform-independent component which accesses control and usage policies across the service-mesh and collects telemetry data from the envoy proxy and other services. 

##  Pilot

Pilot provides services discovery for the Envoy sidecars, traffic management capabilities for intelligent routing


##  Istio Installation

    1.  curl -L https://git.io/getLatestIstio | sh -
    
    2.  Add corresponding path to $PATH
    
    3.  kubectl apply -f install/kubernetes/helm/istio/templates/crds.yaml

    4.  kubectl create -f install/kubernetes/istio-demo.yaml
                            OR
        
        kubectl apply -f install/kubernetes/istio-demo-auth.yaml

    5.  kubectl get svc -n istio-
    
    6.  kubectl get pods -n istio-system
    
    5.  kubectl edit svc istio-ingressgateway -n istio-system 
    
        Change LoadBalancer to NodePort
    
##  Deploy Application to Istio

    1.  enable the istio-injection by executing the command  -- 
    
        kubectl label namespace default istio-injection=enabled --overwrite
        
    2.  Deploy a dummy application
    
        kubectl run hellodemo --image=harshal0812/istiodemo --port=9095 --image-pull-policy=IfNotPresent
        
    3.  kubectl get pods
        
        hellodemo-8f9b8769c-b8gzw   2/2     Running   0          4s
        
    4.  You should get 2 containers in this Pod 
    
        Container 1 - The actual telnet service that communicates with other services. This is the actual image
        
        Container 2 - This container is the envoy container which acts as a proxy service 
        
    5.  kubectl create -f k8s2.bal
    
    7.  kubectl exec -it hellodemo-8f9b8769c-b8gzw -c hellodemo -- curl http://helloworldservice:9095/helloWorld/sayHello

        Hello, World from service helloWorld !


## Invoking Service outside the mesh 

    1.  we need a Gateway and a Virtual service.
    
    2.  Gateways are used to configure the ports for Envoy,
    
    3.  VirtualServices are used to enable the intelligent routing 
    
    4.  kubectl create -f Gateway.yaml

    5.  kubectl create -f VirtualService.yaml

    6.  A VirtualService can then be bound to a gateway to control --
        
        forwarding of traffic arriving at a particular host or gateway port. 
        
    7.  export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
    
    8.  export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].nodePort}')
    
    9.  kubectl get nodes -o wide

    10. Get any IP address of any node
    
    11. curl 18.209.109.19:$INGRESS_PORT/helloWorld/sayHello


    
    



    

