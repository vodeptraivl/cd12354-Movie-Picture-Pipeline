Step 1. Install tfenv and an appropriate version of Terraform

```bash
git clone https://github.com/tfutils/tfenv.git ~/.tfenv
export PATH="$HOME/.tfenv/bin:$PATH"
source ~/.bashrc
tfenv install 1.3.9
tfenv use 1.3.9
cd /workspace/setup/terraform
terraform init
```

apply terraform

```bash
cd /workspace/setup/terraform
terraform apply

cd setup/terraform
terraform output
```

Step 2. Add GitHub Action user to Kubernetes
 1. Create an EKS cluster
```bash
eksctl create cluster --name pj3-cluster --region us-east-1 --nodegroup-name pj3-nodes --node-type t3.small --nodes 1 --nodes-min 1 --nodes-max 2 
```
2. Update the Kubeconfig
```bash
aws eks --region us-east-1 update-kubeconfig --name pj3-cluster
kubectl config current-context
```
3 .Generate AWS credentials for the IAM user account that GitHub Actions will use to interact with your AWS account.

-Launch the Cloud Gateway and go to the IAM service.
-Under users, you should only see the github-action-user user account
-Click the account and go to Security Credentials
-Under Access keys select Create access key
-Select Application running outside AWS and click Next, then Create access key to finish creating the keys
-On the last page, make sure to copy/paste these keys for storing in Github Secrets


4. setup
```bash
cd setup
./init.sh
```