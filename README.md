# Coworking Space Service Extension
The Coworking Space Service is a set of APIs that enables users to request one-time tokens and administrators to authorize access to a coworking space. This service follows a microservice pattern and the APIs are split into distinct services that can be deployed and managed independently of one another.

For this project, you are a DevOps engineer who will be collaborating with a team that is building an API for business analysts. The API provides business analysts basic analytics data on user activity in the service. The application they provide you functions as expected locally and you are expected to help build a pipeline to deploy it in Kubernetes.

## Getting Started

### Dependencies
#### Environment
1. Udacity Project Workspace

#### Remote Resources
1. AWS CodeBuild - build Docker images remotely
2. AWS ECR - host Docker images
3. Kubernetes Environment with AWS EKS - run applications in k8s
4. AWS CloudWatch - monitor activity and logs in EKS
5. GitHub - pull and clone code

### Setup
#### 1. Install eksctl
aws sts get-caller-identity

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version

#### 2. Create Cluster and connect to kubectl
eksctl create cluster --name my-cluster --region us-east-1 --nodegroup-name my-nodes --node-type t3.small --nodes 1 --nodes-min 1 --nodes-max 2

kubectl version --short

aws eks --region us-east-1 update-kubeconfig --name my-cluster

kubectl config current-context
kubectl config view

#### 3. Configure a Database

kubectl apply -f pvc.yaml
kubectl apply -f pv.yaml
kubectl apply -f postgresql-deployment.yaml

kubectl describe pod postgresql-57b584c5bf-dgcks
kubectl logs postgresql-57b584c5bf-dgcks

kubectl exec -it postgresql-696f67d4f4-x2cxv -- bash

psql -U mostafa -d mostafasdatabase

kubectl apply -f postgresql-service.yaml

#### 4. Running the Analytics Application Locally

export DB_USERNAME=mostafa
export POSTGRES_PASSWORD=mostafaspassword
export DB_PASSWORD=${POSTGRES_PASSWORD}
export DB_HOST=127.0.0.1
export DB_PORT=5433
export DB_NAME=mostafasdatabase

# Update the local package index with the latest packages from the repositories
apt update
# Install a couple of packages to successfully install postgresql server locally
apt install build-essential libpq-dev

pip install --upgrade pip setuptools wheel
pip install -r analytics/requirements.txt --force-reinstall

curl 127.0.0.1:5153/api/reports/daily_usage
curl 127.0.0.1:5153/api/reports/user_visits

#### 4. Docker Image

docker build -t test-coworking-analytics .
docker run test-coworking-analytics
docker run --network="host" test-coworking-analytics

### Project Instructions

curl a0d57a18e223f4121b4b12e393a830e5-1004374627.us-east-1.elb.amazonaws.com:5153/api/reports/user_visits

curl a0d57a18e223f4121b4b12e393a830e5-1004374627.us-east-1.elb.amazonaws.com:5153/api/reports/daily_usage

### Deliverables
1. `Dockerfile`
2. Screenshot of AWS CodeBuild pipeline
3. Screenshot of AWS ECR repository for the application's repository
4. Screenshot of `kubectl get svc`
5. Screenshot of `kubectl get pods`
6. Screenshot of `kubectl describe svc <DATABASE_SERVICE_NAME>`
7. Screenshot of `kubectl describe deployment <SERVICE_NAME>`
8. All Kubernetes config files used for deployment (ie YAML files)
9. Screenshot of AWS CloudWatch logs for the application
10. `README.md` file in your solution that serves as documentation for your user to detail how your deployment process works and how the user can deploy changes. The details should not simply rehash what you have done on a step by step basis. Instead, it should help an experienced software developer understand the technologies and tools in the build and deploy process as well as provide them insight into how they would release new builds.

### Validation

Manual testing confirmed that pods and services are running successfully, while CloudWatch logs verified proper application build pipeline and operation.
