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
1 .Generate AWS credentials for the IAM user account that GitHub Actions will use to interact with your AWS account.
- Launch the Cloud Gateway and go to the IAM service.
- Under users, you should only see the github-action-user user account
- Click the account and go to Security Credentials
- Under Access keys select Create access key
- Select Application running outside AWS and click Next, then Create access key to finish creating the keys
- On the last page, make sure to copy/paste these keys for storing in Github Secrets
- go to settings of your git repo, and setting private key with those access key
```bash 
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
```
2. setup
```bash
cd setup
./init.sh
```
3. setting Acction CI and CD.
can check my file.
4. build CI and CD for generate ECR.

step 3. deployment
needed kustomize 
```bash
brew install kustomize
```

deploy 
get account ID
```bash
 aws sts get-caller-identity --query "Account" --output text
```
- go to settings of your git repo, and setting private key with result
```bash 
AWS_ACCOUNT_ID
AWS_DEFAULT_REGION
```
```bash 
sudo kustomize edit set image backend=371809170716.dkr.ecr.us-east-1.amazonaws.com/backend:latest
/# Apply the manifests to the cluster
sudo kustomize build | sudo kubectl apply -f -
```
- get services 
```bash 
sudo kubectl get services 
response 
NAME         TYPE           CLUSTER-IP       EXTERNAL-IP                                                               PORT(S)        AGE
backend      LoadBalancer   172.20.210.157   aa3dbe2f03ed14b45b1a6088cc1036fc-2020040612.us-east-1.elb.amazonaws.com   80:31797/TCP   15h
```
- go to settings of your git repo, and setting varibles key with EXTERNAL-IP
```bash 
REACT_APP_MOVIE_API_URL
```

setting kustomize and apply to kubectl for frontend
```bash 
sudo kustomize edit set image frontend=371809170716.dkr.ecr.us-east-1.amazonaws.com/frontend:latest
/# Apply the manifests to the cluster
sudo kustomize build | sudo kubectl apply -f -
``` 



get services 
```bash
backend      LoadBalancer   172.20.210.157   aa3dbe2f03ed14b45b1a6088cc1036fc-2020040612.us-east-1.elb.amazonaws.com   80:31797/TCP   13m
frontend     LoadBalancer   172.20.172.102   a17d60326076742ffa8454cccb3a55c0-1854245768.us-east-1.elb.amazonaws.com   80:30716/TCP   6m1s
kubernetes   ClusterIP      172.20.0.1       <none>                                                                    443/TCP        155m
```

FRONTEND URL : http://a17d60326076742ffa8454cccb3a55c0-1854245768.us-east-1.elb.amazonaws.com