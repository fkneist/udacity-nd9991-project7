pipeline {

    environment {
        registry = "fkneist/udacity-project7"
        registryCredential = 'dockerhub_id'
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
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()
                    }
                }
            }
        }

        stage('Clean up') {
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}
