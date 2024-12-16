# k0smotron-exp1

Experiment with K0s/K0smotron CAPI provider

Server bootstrap:

``` sh
apt update
apt upgrade
reboot
apt install docker.io
systemctl enable docker
systemctl start docker
curl --proto '=https' --tlsv1.2 -sSf -L https://install.determinate.systems/nix | sh -s -- install --no-confirm
. /nix/var/nix/profiles/default/etc/profile.d/nix-daemon.sh
nix profile install nixpkgs#cmctl nixpkgs#kind nixpkgs#kubectl nixpkgs#kubernetes-helm nixpkgs#yq-go nixpkgs#clusterctl
clusterctl init --bootstrap k0sproject-k0smotron \
  --control-plane k0sproject-k0smotron \
  --infrastructure k0sproject-k0smotron
wget https://github.com/kubernetes-sigs/cloud-provider-kind/releases/download/v0.4.0/cloud-provider-kind_0.4.0_linux_amd64.tar.gz
tar -xvaf cloud-provider-kind_0.4.0_linux_amd64.tar.gz cloud-provider-kind
kind create cluster
./cloud-provider-kind
```
