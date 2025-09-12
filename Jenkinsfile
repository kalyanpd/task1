pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = '1234'   // Jenkins credentials ID
        DOCKER_REGISTRY = 'project'
	DOCKER_USER = 'kalyan3599'              // Default DockerHub registry
        IMAGE_NAME = 'static-site'                 // Image repo name
        IMAGE_TAG = 'latest'                       // Default tag
    }

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
                    sh 'docker rm -f static-site || true'
                    sh 'docker run -d --name static-site -p 8085:80 static-site:latest'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: '1234',
                                                      usernameVariable: 'DOCKER_USER',
                                                      passwordVariable: 'DOCKER_PASS')]) {
                        sh """
                            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                            docker tag static-site:latest $DOCKER_USER/static-site:latest
                            docker push $DOCKER_USER/static-site:latest
                        """
                    }
                }
            }
        }
    }
}

