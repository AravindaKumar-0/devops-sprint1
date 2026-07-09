pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t sprint1:v1 .'
            }
        }
    }
}
