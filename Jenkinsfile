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

        stage('Lint with hadolint') {
            steps {
                sh 'hadolint Dockerfile'
            }
        }
  
        stage('Build Docker image') {
            steps {
                echo 'Building the Docker container...'
                script {
                    // dockerImage = docker.build("${registry}:${currentBuild.number}", "-f Dockerfile .")
                    dockerImage = docker.build("${registry}:latest", "-f Dockerfile .")
                }
            }
        }


        stage('Push image to Docker Hub') {
            steps{
                script {
                    docker.withRegistry('', dockerhubCredentials) {
                        app.push("${env.GIT_COMMIT}")
                        app.push("latest")
                    }
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                    withAWS(credentials: awsCredentials, region: 'us-west-2') {
                        sh 'echo "Setup Kubernetes Cluster"'
                        sh "aws eks --region us-west-2 update-kubeconfig --name UdacityProject7-EKS-CLUSTER"
                        sh 'echo "Deploy to Kubernetes"'
                        sh "kubectl apply -f kubernetes/deployment.yml"
                        sh 'echo "Deployment:"'
                        sh "kubectl get svc"
                        sh "kubectl get pods -o wide"
                        sh "echo 'Completed'"                
                }
            }
        }

        stage('Clean up') {
            steps{
                // sh "docker rmi $registry:${currentBuild.number}"
                sh "docker rmi $registry:latest"
            }
        }   
    }
}
