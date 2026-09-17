pipeline {
    agent any

    stages {

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

        stage('Docker Build') {
            steps {
                sh 'docker build -t cloud-engineer-cicd:${BUILD_NUMBER} .'
            }
        }
    }
}
