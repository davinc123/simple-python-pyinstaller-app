pipeline {
    agent any

    environment {
        IMAGE_NAME = 'hello-app'
        IMAGE_TAG = "${env.BUILD_ID}"
        FULL_IMAGE_NAME = "${IMAGE_NAME}:${IMAGE_TAG}"
        LATEST_IMAGE_NAME = "${IMAGE_NAME}:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${FULL_IMAGE_NAME}")
                    sh "docker tag ${FULL_IMAGE_NAME} ${LATEST_IMAGE_NAME}"
                }
            }
        }

        stage('Test Docker Image') {
            steps {
                script {
                    sh "docker run --rm ${LATEST_IMAGE_NAME}"
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image built and tested successfully!'
            echo "Image: ${FULL_IMAGE_NAME}"
            echo "Latest Image: ${LATEST_IMAGE_NAME}"
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            cleanWs()
        }
    }
}
