# Udacity Cloud DevOps Engineer - Project 7

## Design Decisions
* Rolling deployment or blue/green deployment: blue/green deployment
* AWS Kubernetes as a Service, or build your own Kubernetes cluster: AWS Kubernetes as a Service

## Deploy Instructions

### Create Network:

* `scripts/create-stack.sh <stack_name> <network_yaml> <network_json> <your_aws_profile_name>`
* E.g. `scripts/create-stack.sh project7-network-stack infrastructure/network/network.yml infrastructure/network/network.json fkneist-private`

### Delete Network:

* `scripts/delete-stack.sh <stack_name> <your_aws_profile_name>`
* E.g. `scripts/delete-stack.sh project7-network-stack fkneist-private`

### Create IAM (Jenkins)

* `scripts/create-stack.sh <stack_name> <iam_yaml> <iam_json> <your_aws_profile_name>`
* E.g. `scripts/create-stack.sh project7-iam-stack infrastructure/jenkins/iam.yml infrastructure/jenkins/iam.json fkneist-private`

### Delete IAM (Jenkins)

* `scripts/delete-stack.sh <stack_name> <your_aws_profile_name>`
* E.g. `scripts/delete-stack.sh project7-iam-stack fkneist-private`

### Create Jenkins Server

* `scripts/create-stack.sh project7-jenkins-stack <server_yaml> <server_json> <your_aws_profile_name>`
* E.g. `scripts/create-stack.sh project7-jenkins-stack infrastructure/jenkins/server.yml infrastructure/jenkins/server.json fkneist-private`

### Delete Jenkins Server

* `scripts/delete-stack.sh <server_name> <server_yaml> <server_json> <your_aws_profile_name>`
* E.g. `scripts/delete-stack.sh project7-jenkins-stack fkneist-private`

### Setup Jenkins
* Get `<EC2_instance_public_ip>` from AWS Console
* SSH into EC2 instance: `ssh -i <pem_file> ubuntu@<EC2_instance_public_ip>`
* Get password: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
* Visit `<EC2_instance_public_ip>:8080` in browser, set up user with above password
* Install recommended plugins
* Install plugins: `Blue Ocean Aggregator`, `Pipeline: AWS Steps`
* Visit `<EC2_instance_public_ip>:8080/blue` in browser
* Create new project, `Where do you store your code?`, select GitHub, enter token, select profile, select project `udacity-nd9991-project7`, press `Create Pipeline`
* Add Docker Hub credentials to Jenkins
* Add AWS credentials to Jenkins


## Start the Container locally
* `docker run --name p7-ngingx-container -p 8080:80 project7-nginx`
