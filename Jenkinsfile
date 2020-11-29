pipeline {

    environment {
        registry = "fkneist/udacity-project7"
        registryCredentials = 'dockerhub_id'
        awsCredentials = 'aws_credentials_id'
        awsRegion = 'us-west-2'
        dockerImage = ''
    }
    agent any
    stages {
  
        stage('Build Docker image') {
            steps {
                echo 'Building the Docker container...'
                script {
                    dockerImage = docker.build("${registry}:${currentBuild.number}", "-f Dockerfile .")
                }
            }
        }


        stage('Push image to Docker Hub') {
            steps{
                script {
                    docker.withRegistry( '', registryCredentials ) {
                    dockerImage.push()
                    }
                }
            }
        }

        stage('Create kubernetes configuration') {
            steps {
                withAWS(region: awsRegion, credentials: awsCredentials) {
                sh '''
                        aws eks --region us-west-2 update-kubeconfig --name project7
                    '''
                }
            }
        }

        stage('Deploy to EKS') {
            // steps {
            //     echo 'Deploying to EKS cluster'
            //     withAWS(region: 'us-west-2', credentials: awsCredentials) {
            //         sh 'aws eks --region us-west-2 update-kubeconfig --name project7'
            //         withCredentials([string(credentialsId: 'aws_account_id', variable: 'AWS_ACCOUNT_ID')]) { 
            //             sh "kubectl config use-context arn:aws:eks:us-west-2:${AWS_ACCOUNT_ID}:cluster/project7"
            //         }
            //         sh "kubectl set image deployment/project7-deployment project7=fkneist/udacity-project7:${currentBuild.number} --record"
            //         sh 'kubectl apply -f project7-deployment.yml --force=true'
            //         sh 'kubectl get pods'
            //         sh 'kubectl get services'
            //     }

            // }
            steps {
                    withAWS(credentials: awsCredentials, region: 'us-west-2') {
                        sh 'echo "Setup Kubernetes Cluster"'
                        sh "aws eks --region us-west-2 update-kubeconfig --name UdacityProject7-EKS-CLUSTER"
                        sh 'echo "Deploy to Kubernetes"'
                        sh "kubectl apply -f kubernetes/deployment.yml"
                        sh 'echo "Deployment:"'
                        sh "kubectl get svc"
                        sh "kubectl get pods -o wide"
                        script{
                            serviceAddress = sh(script: "kubectl get svc --output=json | jq -r '.items[0] | .status.loadBalancer.ingress[0].hostname'", returnStdout: true).trim()
                        }
                        sh "echo 'Completed'"
                        sh "echo 'URL: http://$serviceAddress:8000'"
                }
            }
        }

        stage('Clean up') {
            steps{
                sh "docker rmi $registry:latest"
            }
        }

        
    }
}
