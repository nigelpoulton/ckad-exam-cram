# README

Complete the following steps to build a lab environment. They may look complicated, but they're not.

1. Install `kubectl` and `git`
2. Sign-up to Linode
3. Create a Kubernetes cluster on Linode
4. Connect to your Kubernetes cluster
5. Build your lab environment

## Install `kubectl` and `git`

**Install kubectl**

Instructions for [Mac](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/#install-with-homebrew-on-macos)

Instructions for [Windows](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/#install-kubectl-on-windows)

If you need to update your PATH variable:

- MacOS: 
- Windows: 

**Install git**

<coming soon>

You will probably need to quit and re-open your terminal.

## Sign-up to Linode

Go to [linode.com](https://linode.com) and sign-up with your coupon code...

## Create a Kubernetes cluster on Linode

You must be logged in to linode.com to complete the following steps.

1. Select **Kubernetes** on the left-hand navigation pane
2. Choose `Create Cluster` and set the following options
3. **Cluster label:** Give your cluster a name such as **ckad**
4. **Region:** Fremont, CA
5. **Kubernetes Version:** 1.23
6. **Add Node Pools:**  Click `Dedicated CPU` and click the blue `Add` button at the end of the **Dedicated 4 GB** line
7. When your settings match the image below, click `Create Cluster`

![LKE Settings](img/lke.png)

It may take a couple of minutes for your cluster to be created.

## Connect to your Kubernetes cluster

You must be logged in to linode.com to complete the following steps.

1. Click **Kubernetes** on the left-hand navigation pane
2. Click the `Download kubeconfig` link for your cluster
3. Copy the downloaded file to the hidden `./kube` folder in your home directory
    - If you already have a file called `config` in your hidden `./kube` folder, rename the existing file to `config.bkp`
    - Rename the file you just downloaded from Linode and copied to your hidden `./kube` folder to `config`. Be sure the file is called `config` and **not** something like `config.yml`.
4. Open a terminal and type the following command. Your output should look similar

```
kubectl get nodes
NAME                           STATUS   ROLES    AGE     VERSION
lke76472-118784-634d2eb30838   Ready    <none>   5m23s   v1.23.6
lke76472-118784-634d2eb32f20   Ready    <none>   5m21s   v1.23.6
lke76472-118784-634d2eb354b1   Ready    <none>   4m51s   v1.23.6
```

**Congratulations, you're connected to your Kubernetes cluster.**

## Build your lab environment

Open a terminal and run the following commands to build your lab environment.

You must have `kubectl` and `git` installed to complete these steps. You must also be connected to the Kubernetes cluster your just created on Linode.

**Deploy an NGINX ingress controller**

`kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.3.0/deploy/static/provider/cloud/deploy.yaml`

**Clone the lab configuration files from GitHub**
It is recommended to run the following command from the root of your home folder or a temp directory.

`git clone https://github.com/nigelpoulton/ckad-exam-cram.git`

**Change into the 'ckad-exam-cram' directory**

`cd ckad-exam-cram`

**Build your lab**

Run the following command from within the `ckad-exam-cram` directory.

`kubectl apply -f ckad-lab.yml`

If the command returns an error deploying the `app.com` Ingress resource, wait two minutes and try again. This is usually because the NGINX controller is still installing.

**Check your lab**

Run the following commands. If your outputs look the same, your lab is ready to go.

```
kubectl get ingressclass
NAME    CONTROLLER             PARAMETERS   AGE
nginx   k8s.io/ingress-nginx   <none>       4m
```

```
kubectl get ns
NAME              STATUS   AGE
ckad-cram         Active   33s
ckad-ftw          Active   30s
ckad002           Active   34s
ckad003           Active   33s
cram001           Active   34s
db001             Active   32s
default           Active   93m
detroit           Active   31s
exam-ckad         Active   34s
ingress-nginx     Active   86m
kube-node-lease   Active   93m
kube-public       Active   93m
kube-system       Active   93m
kubecon           Active   31s
pluralsight       Active   35s
sa001             Active   32s
```