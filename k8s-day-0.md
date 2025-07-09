
# Step - 1 : Create EKS Management Host in AWS #

1) Launch new Ubuntu VM using AWS Ec2 ( t2.micro )	  
2) Connect to machine and install kubectl using below commands  
```
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
```
![WhatsApp Image 2025-06-26 at 20 09 31_d136038e](https://github.com/user-attachments/assets/92c9e054-5bb2-410b-a545-e65db81725eb)

3) Install AWS CLI latest version using below commands 
```
sudo apt install unzip
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```
![WhatsApp Image 2025-06-26 at 20 14 04_ac70f046](https://github.com/user-attachments/assets/dbf05d1a-f752-40e7-bb8d-f873458c967a)


4) Install eksctl using below commands
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```
![WhatsApp Image 2025-06-26 at 20 15 17_f8317a13](https://github.com/user-attachments/assets/1c9603e9-f0d0-43fb-a412-be0aca2bb6a2)

# Step - 2 : Create IAM role & attach to EKS Management Host #

1) Create New Role using IAM service ( Select Usecase - ec2 ) 	
2) Add below permissions for the role <br/>
	- Administrator - acces <br/>
		
3) Enter Role Name (eksroleec2) 
4) Attach created role to EKS Management Host (Select EC2 => Click on Security => Modify IAM Role => attach IAM role we have created) 
![WhatsApp Image 2025-06-26 at 20 16 02_116579f3](https://github.com/user-attachments/assets/7f2b4f42-c6cc-4b4f-bdd5-774d2daeac1e)
![WhatsApp Image 2025-06-26 at 20 16 33_24dfa57e](https://github.com/user-attachments/assets/f9b331d2-6c1d-4ca8-bfc6-e090a1d8d49b)
![WhatsApp Image 2025-06-26 at 20 16 53_2344b13b](https://github.com/user-attachments/assets/74a751ba-93a0-4470-81d6-a0931aaff1aa)



# Step - 3 : Create EKS Cluster using eksctl # 
**Syntax:** 

eksctl create cluster --name cluster-name  \
--region region-name \
--node-type instance-type \
--nodes-min 2 \
--nodes-max 2 \ 
--zones <AZ-1>,<AZ-2>

## N. Virgina: <br/>
`
eksctl create cluster --name aditya-cluster4 --region us-east-1 --node-type t2.medium  --zones us-east-1a,us-east-1b
`	
## Mumbai: <br/>
`
eksctl create cluster --name aditya-cluster4 --region ap-south-1 --node-type t2.medium  --zones ap-south-1a,ap-south-1b
`

## Note: Cluster creation will take 5 to 10 mins of time (we have to wait). After cluster created we can check nodes using below command.

`
 kubectl get nodes  
`

# Note: We should be able to see EKS cluster nodes here.**

# We are done with our Setup #
	
# Step - 4 : After your practise, delete Cluster and other resources we have used in AWS Cloud to avoid billing #

```
eksctl delete cluster --name aditya-cluster4 --region ap-south-1
```
