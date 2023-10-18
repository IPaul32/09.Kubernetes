# 09. Kubernetes

## KinD Kubernetes Cluster Setup
 
### Install Docker and KinD on your local machine.
Before Install KinD, I need Install GO:
```bash
 wget https://go.dev/dl/go1.21.3.linux-amd64.tar.gz
    4  mkdir -p $HOME/go/{bin,src}
    5  cd
    6  ls -la
    7  nano .profile
    8  cd /home
    9  tar -xvf go1.21.3.linux-amd64.tar.gz
   10  mv go /usr/local/
   11  cd
   12  . ~/.profile
   13  go version
```
```bsah
 go version
go version go1.21.3 linux/amd64
```

After that I can install KinD:
```bash 
 14  go install sigs.k8s.io/kind@v0.20.0
```
```bash
kind version
kind v0.20.0 go1.21.3 linux/amd64
```

###Create a KinD cluster with a single control plane node.
This is where I had an error on step  `✓ Preparing nodes 📦`
`ERROR: failed to create cluster:` 
`failed to init node with kubeadm: command "docker exec --privileged kind-control-plane kubeadm init` 
`--skip-phases=preflight --config=/kind/kubeadm.conf --skip-token-print --v=6" failed with error: exit status 1`
I found solution in ofitial website/ I han cgroup.v2. But I need cgroup.v1
```bash
nano /etc/default/grub
sudo update-grub
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-5.10.0-26-amd64
Found initrd image: /boot/initrd.img-5.10.0-26-amd64
Found linux image: /boot/vmlinuz-5.10.0-18-amd64
Found initrd image: /boot/initrd.img-5.10.0-18-amd64
Warning: os-prober will be executed to detect other bootable partitions.
Its output will be used to detect bootable binaries on them and create new boot                                                                                                              entries.
done
 docker info | grep Cgroup
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
```

**Create cluster:**
```bash
kind create cluster
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.27.3) 🖼
 ✓ Preparing nodes 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
Set kubectl context to "kind-kind"
You can now use your cluster with:

kubectl cluster-info --context kind-kind

Have a nice day! 👋
```

### Verify that the cluster is up and running using kubectl.
Before verifying I need to install `kubectl`:
```bash
  56  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  57  sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

After than I can use him:
```bash 
kubectl version --client
Client Version: v1.28.3
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
➜  09.Kubernetes kubectl get nodes
NAME                 STATUS   ROLES           AGE   VERSION
kind-control-plane   Ready    control-plane   15m   v1.27.3
➜  09.Kubernetes kubectl get pods -A
NAMESPACE            NAME                                         READY   STATUS    RESTARTS   AGE
kube-system          coredns-5d78c9869d-8q8g6                     1/1     Running   0          15m
kube-system          coredns-5d78c9869d-j7qz6                     1/1     Running   0          15m
kube-system          etcd-kind-control-plane                      1/1     Running   0          15m
kube-system          kindnet-mdjsv                                1/1     Running   0          15m
kube-system          kube-apiserver-kind-control-plane            1/1     Running   0          15m
kube-system          kube-controller-manager-kind-control-plane   1/1     Running   0          15m
kube-system          kube-proxy-jqdsp                             1/1     Running   0          15m
kube-system          kube-scheduler-kind-control-plane            1/1     Running   0          15m
local-path-storage   local-path-provisioner-6bc4bddd6b-xgp7f      1/1     Running   0          14m
```


## Minikube Kubernetes Cluster Setup
Situation. All this time I was working on a VM on Proxmox, which is used as the main OS at work 
(I can do my homework at work and at home). It was Debian. I added the CPU and RAM for the Minikube and installed it. 
Before this I also installed Virtualbox, but it complained that there was no graphical interface and cursed. 
In general, I tried to install a Minikube cluster with a driver from Virtualbox. 
I checked that even the machine has Intel virtualization. Everything should work, but after trying to create 
a cluster, the machine does not work at all. I can't even turn it off. It hangs on the login and reboots itself 
and does not respond to any commands. Therefore, I created a new machine and installed KinD on it all over again.
And I already installed Minikube on my home laptop on Windows, which is why 
I provide screenshots of the installation and verification of the Minikube cluster.

![photo_5296441962079048615_y](https://github.com/IPaul32/sa2-25-23-Ivanchuk/assets/145698867/c7df8543-797a-4c3f-afac-f509681f32f9)

---

![photo_5296441962079048617_y](https://github.com/IPaul32/sa2-25-23-Ivanchuk/assets/145698867/c78fa56c-c391-43e9-9e16-94fe861970d7)

---

![photo_5296441962079048614_y](https://github.com/IPaul32/sa2-25-23-Ivanchuk/assets/145698867/ebe4f96d-d780-41ce-b5e1-48a879a21fc7)

---

## GitHub Actions for KinD Cluster Setup


