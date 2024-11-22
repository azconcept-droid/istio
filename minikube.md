# istio
Istio is an open source service mesh that provides a uniform way to integrate microservices, manage traffic flow across microservices, enforce policies, and aggregate telemetry data.

# Using the Istio Addon

## istio Addon

**istio** - Cloud platforms provide a wealth of benefits for the organizations that use them.

### Enable istio on minikube

Make sure to start minikube with at least 8192 MB of memory and 4 CPUs. See official Platform Setup documentation.
```
minikube start --memory=8192mb --cpus=4

minikube addons enable istio-provisioner
minikube addons enable istio

kubectl get po -n istio-system

minikube addons disable istio-provisioner
minikube addons disable istio
```



