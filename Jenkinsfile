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
                    dockerImage = docker.build(registry, "-f Dockerfile .")
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

        stage('Clean up') {
            steps{
                sh "docker rmi $registry:latest"
            }
        }

        
    }
}
