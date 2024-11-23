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

istioctl analyze

k apply -f samples/addons/

kubectl rollout status deployment/kiali -n istio-system

export PATH=$PWD/bin:$PATH

istioctl dashboard kiali

while sleep 0.01;do curl -sS 'http://'"$INGRESS_HOST"':'"$INGRESS_PORT"'/productpage'\ &> /dev/null ; done

k delete deployment productpage-v1
