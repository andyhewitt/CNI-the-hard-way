# Calico the hard way

```bash
curl -L https://github.com/projectcalico/calico/releases/download/v3.28.0/calicoctl-linux-arm64 -o calicoctl
chmod +x calicoctl
mv calicoctl /usr/local/bin/

docker cp calico-kind-control-plane:/cni.kubeconfig ./cni.kubeconfig

docker cp ./cni.kubeconfig calico-kind-worker:/cni.kubeconfig
docker cp ./cni.kubeconfig calico-kind-worker2:/cni.kubeconfig
docker cp ./cni.kubeconfig calico-kind-worker3:/cni.kubeconfig
docker cp ./cni.kubeconfig calico-kind-worker4:/cni.kubeconfig
```
