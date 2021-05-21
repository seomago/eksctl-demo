
How to create a cluster EKS easily
==
# Resources
https://eksctl.io
https://eksctl.io/usage/schema/

# Install the EKS CLI 
from https://eksctl.io/introduction/#installation

* step 1 Download binary on your local machine
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
* step 2 mv executable to /usr/bin
sudo mv /tmp/eksctl /usr/bin
* check version
eksctl version

# Initial Setup 
* Create IAM User programmatic with policy attached **AdministratorAccess**.
* create env variables
export AWS_ACCESS_KEY_ID=[...]

export AWS_SECRET_ACCESS_KEY=[...]


ls -l $HOME/.kube/config

* Check credentials
eksctl get cluster --region us-east-1



# How NOT to create a cluster EKS 

```
# Do NOT do this!
# eksctl create cluster
```
```
# Do NOT do this!
# eksctl create cluster \
#     --name c1 \
#     --region us-east-1 \
#     --version 1.18 \
#     --nodegroup-name primary \
#     --node-type t2.small \
#     --nodes-min 3 \
#     --nodes-max 6 \
#     --managed \
#     --spot \
#     --asg-access \
#     --full-ecr-access
```

# Creating a AWS EKS cluster #


git clone https://github.com/seomago/eksctl-demo.git
cd eksctl-demo
cat cluster-1.17.yaml

#Open https://eksctl.io/usage/schema/

export KUBECONFIG=$PWD/kubeconfig.yaml
eksctl create cluster \
    --config-file cluster-1.17.yaml

#Exploring the outcomes #

#Open AWS Web Console and make sure that us-east-1 is selected

eksctl get clusters --region us-east-1
kubectl version --short --client
kubectl get nodes
```
root@ubuntu100:~/eksctl-demo# eksctl get cluster
2021-05-20 01:12:28 [ℹ]  eksctl version 0.50.0
2021-05-20 01:12:28 [ℹ]  using region us-east-1
NAME	REGION		EKSCTL CREATED
c1	us-east-1	True

```
```
# kubectl get nodes
NAME                            STATUS   ROLES    AGE   VERSION
ip-192-168-61-30.ec2.internal   Ready    <none>   28m   v1.17.12-eks-7684af
root@ubuntu100:~/eksctl-demo# kubectl create ns test
namespace/test created
root@ubuntu100:~/eksctl-demo# kubectl get ns        
NAME              STATUS   AGE
default           Active   35m
kube-node-lease   Active   35m
kube-public       Active   35m
kube-system       Active   35m
test              Active   7s
```


# How NOT to scale a cluster #

```
# Do NOT do this
# eksctl scale nodegroup \
#     --cluster c1 \
#     --nodes 4 \
#     --name primary
```
```
# Cluster Autoscaler?
# Open https://docs.aws.amazon.com/eks/latest/userguide/cluster-autoscaler.html
```

# Upgrading the control plane #


diff cluster-1.17.yaml cluster-1.18.yaml

eksctl upgrade cluster \
    --config-file cluster-1.18.yaml

eksctl upgrade cluster \
    --config-file cluster-1.18.yaml \
    --approve

kubectl get nodes


# Upgrading worker nodegroups #


eksctl create nodegroup \
    --config-file cluster-1.18.yaml

kubectl get nodes

eksctl delete nodegroup \
    --config-file cluster-1.18.yaml \
    --only-missing

eksctl delete nodegroup \
    --config-file cluster-1.18.yaml \
    --only-missing \
    --approve

kubectl get nodes


# Eksctl add-ons

eksctl utils describe-addon-versions \
    --cluster devops-catalog \
    --region us-east-2


# Destroying the cluster #


eksctl delete cluster \
    --config-file cluster-1.18.yaml \
    --wait

# Youtube Video 
Explaining eksctl cluster creation by vfarcic
step by step
How to Create and Manage AWS EKS clusters 
https://youtu.be/pNECqaxyewQ              