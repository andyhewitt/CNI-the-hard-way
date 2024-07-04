# Calico the hard way

```bash
curl -L https://github.com/projectcalico/calico/releases/download/v3.28.0/calicoctl-linux-amd64 -o calicoctl
chmod +x calicoctl
mv calicoctl /usr/local/bin/

docker cp calico-kind-control-plane:/cni.kubeconfig ./cni.kubeconfig

docker cp ./cni.kubeconfig calico-kind-worker:/cni.kubeconfig
docker cp ./cni.kubeconfig calico-kind-worker2:/cni.kubeconfig
docker cp ./cni.kubeconfig calico-kind-worker3:/cni.kubeconfig
docker cp ./cni.kubeconfig calico-kind-worker4:/cni.kubeconfig
```

```bash
calicoctl --allow-version-mismatch apply -f - <<EOF
kind: BGPPeer
apiVersion: projectcalico.org/v3
metadata:
  name: peer-to-rrs
spec:
  nodeSelector: "!has(calico-route-reflector)"
  peerSelector: has(calico-route-reflector)
EOF

calicoctl --allow-version-mismatch apply -f - <<EOF
kind: BGPPeer
apiVersion: projectcalico.org/v3
metadata:
  name: rrs-to-rrs
spec:
  nodeSelector: has(calico-route-reflector)
  peerSelector: has(calico-route-reflector)
EOF

calicoctl --allow-version-mismatch create -f - <<EOF
 apiVersion: projectcalico.org/v3
 kind: BGPConfiguration
 metadata:
   name: default
 spec:
   nodeToNodeMeshEnabled: false
   asNumber: 64512
EOF
```
