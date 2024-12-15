pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'my-python-app'
        DEPLOY_CONTAINER_NAME = 'python-app-container'
        APP_PORT = '5000'
        DEPLOY_PORT = '8080'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                git branch: 'master', url: 'https://github.com/MirzaShahawar/docker-demo-with-simple-python-app.git'
            }
        }

        stage('Docker Build') {
           steps {
                script {
                    echo 'Building Docker image with BuildKit...'
                    sh "DOCKER_BUILDKIT=1 docker build -t ${DOCKER_IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying Docker container...'

                    // Stop and remove the existing container if it exists
                    sh "docker rm -f ${DEPLOY_CONTAINER_NAME} || true"

                    // Run the new container
                    sh "docker run -d --name ${DEPLOY_CONTAINER_NAME} -p ${DEPLOY_PORT}:${APP_PORT} ${DOCKER_IMAGE_NAME}:latest"
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }

        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
