pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'ls -l'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t static-site:latest .'
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Stop old container if running
                    sh 'docker rm -f static-site || true'
                    // Run new container on port 8080
                    sh 'docker run -d --name static-site -p 8085:80 static-site:latest'
                }
            }
        }
    }
}

