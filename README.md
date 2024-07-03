# Calico the hard way

```bash
docker cp calico-kind-control-plane:/cni.kubeconfig ./cni.kubeconfig

docker cp ./cni.kubeconfig calico-kind-worker:/cni.kubeconfig
```
