pipeline {
    agent any
    stages {
        stage('Hello world') {
            steps {
                sh 'echo "Hello World"'
            }
        }
        
        stage('Build Docker image') {
            steps {
                echo 'Building the Docker container...'
                script {
                    dockerImage = docker.build("fkneist/udacity-project7", "-f Dockerfile .")
                }
            }
        }
    }
}
