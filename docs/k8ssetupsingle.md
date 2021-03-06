![Kubernetes](https://mevijay.com/blog/k8ssetup/k8slogo.png)
# Kubernetes cluster lab with ubuntu 20.04

### Cloud-init-config
If you are using this cloud-init user data file on ubuntu 20.04 it will setup all prerequisite including. apt repo, kernel parameter, kubeadm. Just change the desired kubernetes version.
<script src="https://gist.github.com/sharmavijay86/cf86ca128a166ddd456bf0be1b95e2a6.js"></script>
**Step to follow on all nodes**

``` sudo apt-get update```

``` sudo apt-get install containerd -y```

``` curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add```

``` sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"```

``` sudo apt install kubelet=1.19.0-00 kubeadm=1.19.0-00 kubectl=1.19.0-00 -y```
```
sudo modprobe overlay
sudo modprobe br_netfilter

sudo tee /etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF

sudo sysctl --system
```

**step on master node**

``` sudo kubeadm init --pod-network-cidr=10.244.0.0/16```

``` mkdir -p $HOME/.kube```

``` sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config```

``` sudo chown $(id -u):$(id -g) $HOME/.kube/config```

**join worker node ( step on worker node )**

``` kubeadm join 192.168.122.220:6443 --token gefqt9.oj3kcgubehofxbz8 ```
     ```--discovery-token-ca-cert-hash sha256:a79789ade9c95182522f55b1ab17e93cd6eac9c7eaf8b7b67a6c125bbb5f50ce ```

**deploy a pod network plugin ( on master node )**

``` sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml```
```
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```
 
 ### Setup of metal LB (Optional)
Apply deployment manifests-
```
kubectl get configmap kube-proxy -n kube-system -o yaml | sed -e "s/strictARP: false/strictARP: true/" | kubectl apply -f - -n kube-system
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.6/manifests/namespace.yaml
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.6/manifests/metallb.yaml
kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"
```
Create yaml for ip pool 
```
vim ip-pool.yaml
```
Apply the ip pool for LB. Create and modify values based on your network.
```
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 192.168.122.20-192.168.122.30
```
Apply
```
kubectl apply -f ip-pool.yaml
```
## Setup ingres as nginx
 - Daemonset
 ``` 
 helm install ingress-nginx ingress-nginx/ingress-nginx --namespace=ingress --create-namespace=true \
    --set controller.kind=DaemonSet,controller.service.enabled=false \
    --set controller.hostNetwork=true,controller.publishService.enabled=false
 ```
 - Deployment
 ```
 helm install ingress-nginx ingress-nginx/ingress-nginx --namespace=ingress --create-namespace=true 
 ```

## NFS dynamic provisioner setup ( Helm Chart )
```
helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/
helm install nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
    --namespace=kube-system \
    --set nfs.server=192.168.1.11 \
    --set nfs.path=/k8snfs/nfs
```
If you wish to set the storage class as default as well Then upgrade the chart
```
helm upgrade nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner     \
     --namespace=kube-system    \
     --set nfs.server=192.168.1.11  \
     --set nfs.path=/k8snfs/nfs  \
     --set storageClass.defaultClass=true
 ```
## Setup Cert-manager
```
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.3.1/cert-manager.yaml
```
#### LE Cluster issuer
```
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
spec:
  acme:
    email: user@example.com
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      name: example-issuer-account-key
    solvers:
    - http01:
        ingress:
          class: nginx
 ```
 ### Self-Signed issuer
 ```
 apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: selfsigned-issuer
  namespace: sandbox
spec:
  selfSigned: {}
---
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: selfsigned-cluster-issuer
spec:
  selfSigned: {}
```
 
 
 
 
 
 
