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

## troubleshooting steps and output

k0s status

```
Version: v1.27.2+k0s.0
Process ID: 21
Role: controller
Workloads: true
SingleNode: false
Kube-api probing successful: false
Kube-api probing last error:  failed to create kube-api client required for kube-api status reports, probably kubelet failed to init: timed out waiting for the condition
```

ps

```
/ # ps wufax | cat
PID   USER     TIME  COMMAND
    1 root      0:00 /sbin/tini -- /bin/sh /entrypoint.sh /k0smotron-entrypoint.sh
    7 root      0:00 {busybox} ash /k0smotron-entrypoint.sh
   21 root      1:28 k0s controller --enable-worker --no-taints --config=/etc/k0s/k0s.yaml --enable-dynamic-config
  111 root      8:13 /var/lib/k0s/bin/kube-apiserver --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname --kubelet-client-key=/var/lib/k0s/pki/apiserver-kubelet-client.key --requestheader-client-ca-file=/var/lib/k0s/pki/front-proxy-ca.crt --authorization-mode=Node,RBAC --kubelet-certificate-authority=/var/lib/k0s/pki/ca.crt --allow-privileged=true --service-account-jwks-uri=https://kubernetes.default.svc/openid/v1/jwks --enable-admission-plugins=NodeRestriction --tls-private-key-file=/var/lib/k0s/pki/server.key --v=1 --endpoint-reconciler-type=none --enable-bootstrap-token-auth=true --profiling=false --proxy-client-key-file=/var/lib/k0s/pki/front-proxy-client.key --requestheader-group-headers=X-Remote-Group --requestheader-allowed-names=front-proxy-client --client-ca-file=/var/lib/k0s/pki/ca.crt --kubelet-client-certificate=/var/lib/k0s/pki/apiserver-kubelet-client.crt --service-account-signing-key-file=/var/lib/k0s/pki/sa.key --tls-cipher-suites=TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256,TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256 --service-account-issuer=https://kubernetes.default.svc --advertise-address=10.68.1.37 --secure-port=6443 --proxy-client-cert-file=/var/lib/k0s/pki/front-proxy-client.crt --service-cluster-ip-range=10.20.0.0/16 --tls-cert-file=/var/lib/k0s/pki/server.crt --feature-gates= --requestheader-username-headers=X-Remote-User --tls-min-version=VersionTLS12 --anonymous-auth=false --requestheader-extra-headers-prefix=X-Remote-Extra- --egress-selector-config-file=/var/lib/k0s/konnectivity.conf --api-audiences=https://kubernetes.default.svc,system:konnectivity-server --service-account-key-file=/var/lib/k0s/pki/sa.pub --etcd-servers=https://kmc-cpo127-k0swc1-etcd:2379 --etcd-cafile=/var/lib/k0s/pki/etcd-ca.crt --etcd-certfile=/var/lib/k0s/pki/apiserver-etcd-client.crt --etcd-keyfile=/var/lib/k0s/pki/apiserver-etcd-client.ke
  156 root      0:01 /usr/local/bin/k0s api --data-dir=/var/lib/k0s
  168 root      0:37 /var/lib/k0s/bin/kube-scheduler --feature-gates= --bind-address=127.0.0.1 --leader-elect=true --profiling=false --authentication-kubeconfig=/var/lib/k0s/pki/scheduler.conf --authorization-kubeconfig=/var/lib/k0s/pki/scheduler.conf --kubeconfig=/var/lib/k0s/pki/scheduler.conf --v=1
  169 root      2:47 /var/lib/k0s/bin/kube-controller-manager --controllers=*,bootstrapsigner,tokencleaner --enable-hostpath-provisioner=true --requestheader-client-ca-file=/var/lib/k0s/pki/front-proxy-ca.crt --allocate-node-cidrs=true --bind-address=127.0.0.1 --cluster-name=k0s --use-service-account-credentials=true --terminated-pod-gc-threshold=12500 --authentication-kubeconfig=/var/lib/k0s/pki/ccm.conf --kubeconfig=/var/lib/k0s/pki/ccm.conf --feature-gates= --cluster-signing-cert-file=/var/lib/k0s/pki/ca.crt --cluster-signing-key-file=/var/lib/k0s/pki/ca.key --profiling=false --authorization-kubeconfig=/var/lib/k0s/pki/ccm.conf --client-ca-file=/var/lib/k0s/pki/ca.crt --service-cluster-ip-range=10.20.0.0/16 --v=1 --node-cidr-mask-size=24 --leader-elect=true --root-ca-file=/var/lib/k0s/pki/ca.crt --service-account-private-key-file=/var/lib/k0s/pki/sa.key --cluster-cidr=10.10.0.0/16
  182 root      0:00 /var/lib/k0s/bin/konnectivity-server --kubeconfig=/var/lib/k0s/pki/konnectivity.conf --v=1 --cipher-suites=TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256,TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256 --server-count=1 --mode=grpc --agent-service-account=konnectivity-agent --delete-existing-uds-file=true --authentication-audience=system:konnectivity-server --enable-profiling=false --agent-port=8132 --agent-namespace=kube-system --stderrthreshold=1 --cluster-key=/var/lib/k0s/pki/server.key --uds-name=/run/k0s/konnectivity-server/konnectivity-server.sock --cluster-cert=/var/lib/k0s/pki/server.crt --admin-port=8133 --server-id=378056a96b72c00387f0f4988c5eaa256257adf32a5c99c6a3d3b12688fd356c --server-port=0 --logtostderr=true --proxy-strategies=destHost,default
```

kubectl

```
/ # k0s kubectl get nodes
No resources found
/ # k0s kubectl get -A pods
NAMESPACE     NAME                              READY   STATUS    RESTARTS   AGE
kube-system   coredns-878bb57ff-d4vs4           0/1     Pending   0          6m25s
kube-system   metrics-server-7f86dff975-w9bv9   0/1     Pending   0          6m25s
```
