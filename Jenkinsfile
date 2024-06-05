pipeline {
    agent any
    
    environment {
        ROLL_NUMBER = "1159"
        IMAGE_NAME = "harisathar04/online-classroom"
        DOCKER_REGISTRY_CREDENTIALS = 'docker-hub-credentials'
    }

    stages {
        stage('21i-${ROLL_NUMBER}_Checkout Code') {
            steps {
                echo "Checking out code from GitHub repository..."
                git 'https://github.com/NUCESFAST/scd-final-lab-exam-harisathar04.git'
            }
        }

        stage('21i-${ROLL_NUMBER}_Build Docker Images') {
            steps {
                script {
                    echo "Building Docker images for each component..."
                    echo "Building client Docker image..."
                    docker.build("${IMAGE_NAME}-client:${BUILD_NUMBER}", './client')
                    
                    echo "Building classrooms Docker image..."
                    docker.build("${IMAGE_NAME}-classrooms:${BUILD_NUMBER}", './backend/Classrooms')
                    
                    echo "Building auth Docker image..."
                    docker.build("${IMAGE_NAME}-auth:${BUILD_NUMBER}", './backend/Auth')
                    
                    echo "Building post Docker image..."
                    docker.build("${IMAGE_NAME}-post:${BUILD_NUMBER}", './backend/Post')
                    
                    echo "Building event-bus Docker image..."
                    docker.build("${IMAGE_NAME}-event-bus:${BUILD_NUMBER}", './backend/event-bus')
                }
            }
        }

        stage('21i-${ROLL_NUMBER}_Push Docker Images') {
            steps {
                script {
                    echo "Pushing Docker images to registry..."
                    docker.withRegistry('', DOCKER_REGISTRY_CREDENTIALS) {
                        dockerImagePush("${IMAGE_NAME}-client:${BUILD_NUMBER}")
                        dockerImagePush("${IMAGE_NAME}-classrooms:${BUILD_NUMBER}")
                        dockerImagePush("${IMAGE_NAME}-auth:${BUILD_NUMBER}")
                        dockerImagePush("${IMAGE_NAME}-post:${BUILD_NUMBER}")
                        dockerImagePush("${IMAGE_NAME}-event-bus:${BUILD_NUMBER}")
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

def dockerImagePush(imageTag) {
    echo "Pushing Docker image: ${imageTag}"
    dockerImage.push("${imageTag}")
    dockerImage.push("${imageTag}".replace(':latest', ':${BUILD_NUMBER}'))
}