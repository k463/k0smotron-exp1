# cp-only-127

Minimal Cluster with only the Control Plane managed through the
`K0smotronControlPlane` resource.

Initial bootstrap steps [here](../README.md), then:

``` sh
git clone https://github.com/k463/k0smotron-exp1.git
cd k0smotron-exp1
kubectl apply -f cp-only-127/0ns.yaml
kubectl apply -f cp-only-127/k0smotron-control-plane.yaml
kubectl apply -f cp-only-127/remote-cluster.yaml
kubectl apply -f cp-only-127/cluster.yaml
```
