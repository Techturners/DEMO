pipeline {
    agent any

    environment {
        // Updated for your project naming convention
        IMAGE_NAME = "techturners/demo-app" 
        IMAGE_TAG = "${BUILD_NUMBER}" // Uses Jenkins build number for unique tags
    }

    stages {
        stage('Initial Check') {
            steps {
                echo "Starting CI/CD Pipeline for Techturners DEMO project..."
            }
        }

        stage('Checkout Code') {
            steps {
                // Updated to your specific repo and main branch
                git branch: 'main', url: 'https://github.com/Techturners/DEMO.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image: ${IMAGE_NAME}:${IMAGE_TAG}"
                    // This looks for the 'Dockerfile' in your project root
                    def customImage = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Ensure 'dockerhub-creds' exists in Jenkins -> Credentials
                    docker.withRegistry('https://index.docker.io/v1/', '1234') {
                        docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push()
                        // Also pushing a 'latest' tag for convenience
                        docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push("latest")
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Successfully built and pushed ${IMAGE_NAME}:${IMAGE_TAG} to Docker Hub! 🚀"
        }
        failure {
            echo "Pipeline failed for Techturners/DEMO. Check the console logs. ❌"
        }
    }
}