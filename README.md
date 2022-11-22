# Yaml-file-for-spring-application-in-kubernetes
A resource quota, defined by a ResourceQuota object, provides constraints that limit aggregate resource consumption per namespace. It can limit the quantity of objects that can be created in a namespace by type, as well as the total amount of compute resources that may be consumed by resources in that namespace.

Resource quotas work like this:

Different teams work in different namespaces. This can be enforced with RBAC.

The administrator creates one ResourceQuota for each namespace.

Users create resources (pods, services, etc.) in the namespace, and the quota system tracks usage to ensure it does not exceed hard resource limits defined in a ResourceQuota.

If creating or updating a resource violates a quota constraint, the request will fail with HTTP status code 403 FORBIDDEN with a message explaining the constraint that would have been violated.

If quota is enabled in a namespace for compute resources like cpu and memory, users must specify requests or limits for those values; otherwise, the quota system may reject pod creation. Hint: Use the LimitRanger admission controller to force defaults for pods that make no compute resource requirements.




----------
# kops-kubernetes-cluster-configuration
# Landmark Technologies,  -    Landmark Technologies 
# Tel: +1 437 215 2483,   -     +1 437 215 2483 
# mylandmarktech@gaIL.com,  -    www.mylandmarktech.com 

# Setting up Kubernetes (K8s) Cluster on AWS Using KOPS

1.kops is a software use to create production ready k8s cluster in a cloud provider like AWS.

2. kOPS SUPPORTS MULTIPLE CLOUD PROVIDERS

3. Kops compete with managed kubernestes services like EKS, AKS and GKE

4. Kops is cheaper than the others.

5. Kops create production ready K8S.

6. KOPS create resources like: LoadBalancers, ASG, Launch Configuration, woker node Master node (CONTROL PLANE.

7. KOPS is IaaC

#!/bin/bash
# 1) Create Ubuntu EC2 instance in AWS

## 2a) create kops user
``` sh
 sudo adduser kops
 sudo echo "kops  ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/kops
 sudo su - kops
 ```
 ##  2a) install AWSCLI
  ```sh
 sudo apt install awscli -y 
 #or
 sudo apt update -y
 sudo apt install unzip wget -y
 sudo curl https://s3.amazonaws.com/aws-cli/awscli-bundle.zip -o awscli-bundle.zip
 sudo apt install unzip python -y
 sudo unzip awscli-bundle.zip
 sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
 ```
# 3) Install kops software on ubuntu instance:

 	#Install wget if not installed
 	sudo apt install wget -y
 	sudo wget https://github.com/kubernetes/kops/releases/download/v1.22.0/kops-linux-amd64
 	sudo chmod +x kops-linux-amd64
 	sudo mv kops-linux-amd64 /usr/local/bin/kops
 
# 4) Install kubectl
```sh
 sudo curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
 sudo chmod +x ./kubectl
 sudo mv ./kubectl /usr/local/bin/kubectl
```
# 5) Create an IAM role from AWS Console or CLI with below Policies.

	AmazonEC2FullAccess 
	AmazonS3FullAccess
	IAMFullAccess 
	AmazonVPCFullAccess


Then Attach IAM role to ubuntu server from Console Select KOPS Server --> Actions --> Instance Settings --> Attach/Replace IAM Role --> Select the role which
You Created. --> Save.



# 6) create an S3 bucket
# Execute the commands below in your KOPS control Server. use unique s3 bucket name. If you get bucket name exists error.

	aws s3 mb s3://class29v
	aws s3 ls
	
    ex: s3://class29v
     
	Expose environment variable:

    # Add env variables in bashrc
    vi .bashrc
	
	# Give Unique Name And S3 Bucket which you created.
	export NAME=class.k8s.local
	export KOPS_STATE_STORE=s3://class29v
 
    source .bashrc
	
# 7) Create sshkeys before creating cluster

    ssh-keygen
 

# 8) Create kubernetes cluster definitions on S3 bucket

	kops create cluster --zones us-east-2c --networking weave --master-size t2.medium --master-count 1 --node-size t2.medium --node-count=2 ${NAME}
	
	kops create secret --name ${NAME} sshpublickey admin -i ~/.ssh/id_rsa.pub

# 9) Create kubernetes cluser

	 kops update cluster ${NAME} --yes

# 10) Validate your cluster(KOPS will take some time to create cluster ,Execute below commond after 3 or 4 mins)

	   kops validate cluster
	   
	   Suggestions:
 * validate cluster: kops validate cluster --wait 10m
 * list nodes: kubectl get nodes --show-labels
 * ssh to the master: ssh -i ~/.ssh/id_rsa ubuntu@api.class.k8s.local
 * the ubuntu user is specific to Ubuntu. If not using Ubuntu please use the appropriate user based on your OS.
 * read about installing addons at: https://kops.sigs.k8s.io/operations/addons.


# 11) connect to the master node
    sh -i ~/.ssh/id_rsa ubuntu@ipAddress
    ssh -i ~/.ssh/id_rsa ubuntu@18.222.139.125
    ssh -i ~/.ssh/id_rsa ubuntu@172.20.58.124
# 11) To list nodes

	  kubectl get nodes 
 
# 12) To Delete Cluster

   kops delete cluster --name=${NAME} --state=${KOPS_STATE_STORE} --yes  
   
====================================================================================================


13 # IF you want to SSH to Kubernetes Master or Nodes Created by KOPS. You can SSH From KOPS_Server

sh -i ~/.ssh/id_rsa ubuntu@ipAddress
ssh -i ~/.ssh/id_rsa ubuntu@3.90.203.23
  
``
