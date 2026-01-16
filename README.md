# k3s-helmcharts

## Install Ubuntu server on macbook

To install k3s, you first need to set up an Ubuntu Server.
In my case, the setup runs on an old MacBook Pro 13" (2017).

Steps:

* Flash an USB stick with Ubuntu Server
* Install it on the MacBook, overwriting macOS
* Ubuntu Server is ready

---
---
## Install k3s on macbook

* modify clusters/schuk/k3s-config.yaml to change the node name + server ip
* copy it to your server and run 
```
sudo mkdir -p /etc/rancher/k3s
sudo cp infra/k3s-config.yaml /etc/rancher/k3s/config.yaml
```
install k3s with
```
curl -sfL https://get.k3s.io | sh -
```
install flux cli
```
curl -s https://fluxcd.io/install.sh | sudo bash
```
set kubeconfig to variable
```
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
```
log into git
```
#add your server ssh public key with write access to the repo
ssh -T git@github.com
# add ssh agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/{ssh key}
```
setup flux with git repo
```
flux bootstrap git \
  --url=ssh://git@github.com/bladezx/k3s-gitops.git \
  --branch=main \
  --path=clusters/schuk
```
or without repo agent
```
flux bootstrap git \
  --url=ssh://git@github.com/bladezx/k3s-gitops.git \
  --branch=main \
  --path=clusters/schuk \
  --private-key-file=~/.ssh/{ssh key}
```

=> now fluxcd is installed