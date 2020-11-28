# Udacity Cloud DevOps Engineer - Project 7

## Design Decisions
* Rolling deployment or blue/green deployment: blue/green deployment
* AWS Kubernetes as a Service, or build your own Kubernetes cluster: AWS Kubernetes as a Service

## Deploy Instructions

### Create Network:

* `scripts/create-stack.sh <stack_name> <network_yaml> <network_json> <your_aws_profile_name>`
* e.g. `scripts/create-stack.sh project7-network-stack infrastructure/network/network.yml infrastructure/network/network.json fkneist-private`

### Delete Network:

* `scripts/delete-stack.sh <stack_name> <your_aws_profile_name>`
* e.g. `scripts/delete-stack.sh project7-network-stack fkneist-private`

### Create IAM (Jenkins)

* `scripts/create-stack.sh <stack_name> <iam_yaml> <iam_json> <your_aws_profile_name>`
* e.g. `scripts/create-stack.sh project7-iam-stack infrastructure/jenkins/iam.yml infrastructure/jenkins/iam.json fkneist-private`

### Delete IAM (Jenkins)

* `scripts/delete-stack.sh <stack_name> <your_aws_profile_name>`
* e.g. `scripts/delete-stack.sh project7-iam-stack fkneist-private`

## Start the Container locally
* `docker run --name p7-ngingx-container -p 8080:80 project7-nginx`
