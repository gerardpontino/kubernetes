# Deploying a Kubernetes Cluster with K3S
==================

Please refer to [K3S Doccumentation](https://docs.k3s.io/) for more detailed instructions

##### Setting up the server 
------------
Login to the each nodes

```sh
$ ssh root@master

# Set the hostnames of each nodes. In the sample below, I've set the hostname for the master node as "master". You can do the same steps for the worker nodes

$ hostnamectl set-hostname master

# Once all the hostnames are set, add the IP's and hostnames in each hosts. See sample below:

vi /etc/hosts
192.168.18.35 master
192.168.18.52 worker01
192.168.18.53 worker02
192.168.18.54 worker03

```
------------

##### Install K3s in the master

```sh
$ curl -sfL https://get.k3s.io | sh -
```

##### Get the IP and Node Token from the Master node
```sh
iamroot@k3s-master:~ $ hostname -I | awk '{print$1}'
192.168.18.35

```
```sh
iamroot@k3s-master:~ $ sudo cat /var/lib/rancher/k3s/server/node-token
K10910a9f606e89da8a95e3e37ab9faf160a3eeca46229dd82fb902c3984bec8e1b::server:e658625eecb60de3f383ca0a75df3e24
```

##### SSH into every worker node and execute/run the required command(s).
Using the IP and Node Token taken from the Master node. Run curl -sfl https://get.k3s.io |K3S_URL=https://<Master IP>:6443 K3S_TOKEN=<Node Token> sh -
```sh
iamroot@worker01:~ $ curl -sfL https://get.k3s.io | K3S_URL=https://192.168.18.35:6443 K3S_TOKEN=K10910a9f606e89da8a95e3e37ab9faf160a3eeca46229dd82fb902c3984bec8e1b::server:e658625eecb60de3f383ca0a75df3e24 sh -
```
    
Once you have installed the k3s agents on your worker nodes, you can verify that they have been successfully added to the cluster and are in “ready” status. To do this, log in to your master node and run the following command:

```sh
iamroot@master:~ $ sudo kubectl get nodes
NAME         STATUS   ROLES                  AGE    VERSION
k3s-master   Ready    control-plane,master   7d5h   v1.26.3+k3s1
worker02     Ready    <none>                 7d5h   v1.26.3+k3s1
worker01     Ready    <none>                 7d5h   v1.26.3+k3s1
```
