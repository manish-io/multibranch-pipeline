pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = 'abc1b856-9747-4365-87a2-65d902c68afb'
        REPO_NAME = 'manishchauhan27/my-multi-image'
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    echo "Branch: ${env.BRANCH_NAME}"
                    checkout scm
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def imageTag = "${env.BRANCH_NAME}-${env.BUILD_ID}"
                    def image = docker.build("${env.REPO_NAME}:${imageTag}")
                    docker.withRegistry('https://registry.hub.docker.com/', DOCKERHUB_CREDENTIALS) {
                        image.push()
                    }
                }
            }
        }
    }
    post {
        success {
            script {
                echo "Successfully built and pushed the Docker image for branch: ${env.BRANCH_NAME}"
            }
        }
        failure {
            script {
                echo "Failed to build and push the Docker image for branch: ${env.BRANCH_NAME}"
            }
        }
    }
}
