pipeline {
    agent any

    environment {
        IMAGE_NAME = "imaravinda/devops-sprint1"
        IMAGE_TAG = "v1"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push ${IMAGE_NAME}:${IMAGE_TAG}'
            }
        }
    }

    post {
        success {
            echo "Docker image pushed successfully!"
        }

        failure {
            echo "Pipeline Failed!"
        }

        always {
            sh 'docker logout'
        }
    }
}
