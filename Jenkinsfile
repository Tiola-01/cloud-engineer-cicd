pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        ECR_REPOSITORY = 'cloud-engineer-cicd'
        AWS_ACCOUNT_ID = '659161126002'
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}:${IMAGE_TAG}"
    }

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
                sh 'docker build -t ${IMAGE_URI} .'
            }
        }

        stage('ECR Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'aws-ecr',
                        usernameVariable: 'AWS_ACCESS_KEY_ID',
                        passwordVariable: 'AWS_SECRET_ACCESS_KEY'
                    )
                ]) {
                    sh '''
                        export AWS_DEFAULT_REGION=${AWS_REGION}

                        aws ecr get-login-password --region ${AWS_REGION} \
                        | docker login \
                        --username AWS \
                        --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                    '''
                }
            }
        }

        stage('Push to ECR') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'aws-ecr',
                        usernameVariable: 'AWS_ACCESS_KEY_ID',
                        passwordVariable: 'AWS_SECRET_ACCESS_KEY'
                    )
                ]) {
                    sh '''
                        export AWS_DEFAULT_REGION=${AWS_REGION}

                        docker push ${IMAGE_URI}
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "Successfully pushed ${IMAGE_URI}"
        }

        failure {
            echo "Pipeline failed."
        }
    }
}
