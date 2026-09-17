pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Building Cloud Engineer CI/CD application"'
            }
        }

        stage('Test') {
            steps {
                sh 'test -f app/index.html'
                sh 'test -f Dockerfile'
            }
        }
    }
}
