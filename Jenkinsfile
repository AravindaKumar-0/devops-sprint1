pipeline {
    agent any

    environment {
        IMAGE_NAME = "imaravinda/devops-sprint1:v1"
        EC2_HOST = "54.172.217.161"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }

        stage('Deploy to EC2') {

            steps {

                sshagent(credentials: ['ubuntu']) {

                    sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@${EC2_HOST} '

                    docker pull ${IMAGE_NAME}

                    docker stop web || true

                    docker rm web || true

                    docker run -d -p 80:80 --name web ${IMAGE_NAME}

                    '
                    """
                }

            }

        }

    }

    post {

        success {

            sh 'docker logout'

            echo "Deployment Successful 🚀"

        }

        failure {

            sh 'docker logout'

            echo "Deployment Failed ❌"

        }

    }

}
