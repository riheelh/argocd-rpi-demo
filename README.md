## argocd-rpi-demo
This lab shows how to setup and configure Argo CD on a Raspberry Pi cluster. It serves as a hands-on starter kit for playing with GitOps on edge devices.the following to followup with ArgoCD config:

### Hardware Requirements
To follow along with the setup, you will need the following:
- 3 × Raspberry Pi 5 (recommended with heatsinks and cases for cooling)
- LAN switch (minimum 4 ports or higher)
- Cat6 Cables
- USB C Power Chargers for each Pi
- microSD cards (minimum 32GB or higher)

### Step 1 – Install K3s Server Control Plane

We'll install K3s on rpi-node-1 as the control plane.
```shell
ssh rpiadmin@rpi-node-1.local
ssh rpiadmin@rpi-node-2.local
ssh rpiadmin@rpi-node-3.local
curl -sfL https://get.k3s.io | sh -
```

This will:
- Install K3s
- Start the kubernetes control plane
- Setup the kubectl and kubeconfig under /etc/rancher/k3s/k3s.yaml
- validate nodes are working via following commands:

```shell
sudo k3s kubectl get nodes
```

### Step 2 – Join Agent Nodes
First we need to grab the token from rpi-node-1 for nodes to connect
```shell
sudo cat /var/lib/rancher/k3s/server/node-token
```

Next connect to ***rpi-node-2*** and ***rpi-node-3*** and join the them to the K3s cluster, run this command on each agent node
```shell
curl -sfL https://get.k3s.io | K3S_URL=https://<rpi_node_1_ip>:6443 K3S_TOKEN=<TOKEN> sh -
```

After joining check the control plane and agent node using following command:
```shell
sudo k3s kubectl get nodes
```

you should see somthing like this:
```shell
NAME           STATUS   ROLES    AGE     VERSION
rpi-node-1     Ready    control-plane   ...
rpi-node-2     Ready    <none>          ...
rpi-node-3     Ready    <none>          ...
```

when this is done, you have 3 node node K3s cluster running on Raspberry PI