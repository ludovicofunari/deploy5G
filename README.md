# Deploy 5GC using SD-Core and UERANSIM

## Deploy SD-Core
```bash
# Clone SD-Core repository
git clone https://gerrit.opencord.org/sdcore-helm-charts
# Install SD-Core using Helm
helm install sdcore-5g sdcore-helm-charts/sdcore-helm-charts -f my-sdcore-values.yaml
```

## Deploy UERANSIM
```bash
# Clone UERANSIM repository
git clone https://github.com/Orange-OpenSource/towards5gs-helm.git
# Install UERANSIM using Helm
helm install ueransim towards5gs-helm/charts/ueransim -f my-ueransim-values.yaml

# Set route to the gnb:
export POD_GNB=$(kubectl get pods --namespace free5gc -l "component=gnb" -o jsonpath="{.items[0].metadata.name}")
kubectl exec -it $POD_GNB -- apt update && apt install iputils-ping tcpdump iproute2 -y
kubectl exec -it $POD_GNB -- apt install iputils-ping tcpdump iproute2 -y
kubectl exec -it $POD_GNB -- ip r a 192.168.252.0/24 via 192.168.251.1
```

## Test if UE pings the internet

```bash
export POD_UE=$(kubectl get pods --namespace free5gc -l "component=ue" -o jsonpath="{.items[0].metadata.name}")
kubectl --namespace free5gc exec -it $POD_UE -- ping -I uesimtun0 8.8.8.8
```
