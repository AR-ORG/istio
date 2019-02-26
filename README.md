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







    

