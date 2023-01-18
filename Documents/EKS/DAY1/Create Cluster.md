
## Day 01 CLI setup and Creating first EKS Cluster.


🎯 About
Introduction to EKS, and Configurations.


## Features

✔️ Installing CLI and connect with AWS;
✔️ Creating EKS Cluster and Node Groups;
✔️ Deleting Cluster and Node Groups;


## 🚀 Technologies
* [AWS CLI](https://breakdance.github.io/breakdance/)
* [kubectl](https://kubernetes.io/docs/tasks/tools/) - Kubernetes command-line tool
* [eksctl](https://eksctl.io/) - CLI for Amazon EKS


✅ Requirements

Before starting 🏁, you need to have an Active AWS Account and Basic understaing of Kubernetes concepts.


## Part1
(1) Install AWS CLI
-----------------------------
____________________________________
Reference-1: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html
Reference-2: https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html
Validate CLI installation.
```sh
aws --version
aws-cli/1.23.8 Python/3.9.10 Darwin/22.1.0 botocore/1.25.8
which aws
```

(2) Configure AWS Command Line using Security Credentials
---------------------------------------------------------------------
_____________________________________
* Login into AWS account
* Serch for --> IAM in search bar.
* Select the IAM User, Or create new one. Please make sure you never use Root * user's Security credential(Not recommended)
* Click on Security credentials tab
* Click on Create access key
* Copy Access ID and Secret access key
* Go to command line and provide the required details
```sh
aws configure
AWS Access Key ID [None]: paste your's
AWS Secret Access Key [None]: paste your's
Default region name [None]: ap-south-1
Default output format [None]: json
```
* Test if AWS CLI is working after configuring the above
```sh
aws s3 ls
```

(3) Install kubectl CLI
---------------
___________________________________
Reference: https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html
Verify the kubectl client version

```sh
kubectl version --client
```
(4) Install eksctl CLI
-------------------------------------------------------
____________________________
Reference:
* https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html
* https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html

(5) Verify eksctl version
-----------------------------------
_________________________________

```sh
eksctl version
```
Part-2
-------------------------------------------------------------------
______________________________________________________________________
* It will take few minutes to create the Cluster Control Plane
Dillinger is currently extended with the following plugins.
Instructions on how to use them in your own application are linked below.

# Create Cluster
```sh
eksctl create cluster --name=demoeks \
                      --region=ap-south-1 \
                      --zones=ap-south-1a,ap-south-1b \
                      --without-nodegroup 
```


# (2) Creating OpenID Connect (OIDC) identity providers.
___________________________________________________________________
* References:
https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html

```sh
eksctl utils associate-iam-oidc-provider \
    --region ap-south-1 \
    --cluster demoeks \
    --approve
```

(3) Create Public Node Group
-----------------------------------------------
_____________________________________________________________________
```sh
eksctl create nodegroup --cluster=demoeks \
                        --region=ap-south-1 \
                        --name=demoeks-ng- \
                        --node-type=t3.medium \
                        --nodes=2 \
                        --nodes-min=2 \
                        --nodes-max=4 \
                        --node-volume-size=30 \
                        --managed \
                        --asg-access \
                        --external-dns-access \
                        --full-ecr-access \
                        --appmesh-access \
                        --alb-ingress-access 
```

(4) Verify Cluster & Nodes
-------------------------------------
___________________________________________________________

```sh
# List Nodes in current kubernetes cluster
kubectl get nodes -o wide

# Our kubectl context should be automatically changed to new cluster
kubectl config view --minify
```

## Part-3
Deleting Cluster & Node Groups
------------------------------------------------------
_______________________________________________________
Delete Node Group
-
* We can delete a nodegroup separately using below eksctl delete nodegroup
```sh
# List EKS Clusters
eksctl get clusters

# List Node Group
eksctl get nodegroup --cluster=demoeks

# Delete Node Group
eksctl delete nodegroup --cluster=eksdemo1 --name=demoeks-ng-1
```


Delete Cluster
----------------------------------
* We can delete cluster using eksctl delete cluster

```sh
# Delete Cluster
eksctl delete cluster demoeks
```



