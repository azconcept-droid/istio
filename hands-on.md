curl -L https://git.io/getLatestIstio | sh -

cd istio-1.24.0/

export PATH=$PWD/bin:$PATH

istioctl version

istioctl install --set profile=demo -y

kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml

k get pods

istioctl analyze

kubectl label namespace default istio-injection=enabled

kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml 

kubectl exec "$(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl -sS productpage:9080/productpage | grep -o "<title>.*</title>"

istioctl analyze

export INGRESS_HOST=$(minikube ip)

export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')

INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')

curl "http://$INGRESS_HOST:$INGRESS_PORT/productpage"

echo "http://$INGRESS_HOST:$INGRESS_PORT/productpage"

k apply -f samples/addons/

kubectl rollout status deployment/kiali -n istio-system

export PATH=$PWD/bin:$PATH

istioctl dashboard kiali

while sleep 0.01;do curl -sS 'http://'"$INGRESS_HOST"':'"$INGRESS_PORT"'/productpage'\ &> /dev/null ; done

k delete deployment productpage-v1

### Istio gateway
Istio gateway are loadbalancers that sit at the edge of the mesh. Manage the inbound and outbound traffic to the servics. Standalone envoy proxy.

istio system
- Istiod
- istio-ingress gateway
- istio-egress gateway

Create gateway object

### Virtual Services
All routing rules are configured through virtual services. All routing rules coming from the istio gateway goes to the virtual service.

hosts: bookinfo.app
/productpage
/static
/login
/logout

curl -s -HHost:bookinfo.app http://$INGRESS_HOST:$INGRESS_PORT/productpage | grep -o *<title>.*</title>

echo -e "$(minikube ip)\tbookinfo.app" | sudo tee -a /etc/hosts

bookinfo.app:31247/productpage