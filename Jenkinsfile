
pipeline {
    agent any

    environment {
        REGISTRY = "localhost:5000"
        IMAGE_NAME = "anil-demo-app"
        PYTHON = "C:\\Users\\STUDENT\\AppData\\Local\\Programs\\Python\\Python313\\python.exe"
    }

    stages {

        stage('1. Checkout Code') {
            steps {
                checkout scm
                echo 'Source code downloaded successfully!'
            }
        }

        stage('2. Application Build & Test') {
            steps {
                bat '''
                    "%PYTHON%" --version
                    "%PYTHON%" -m py_compile app.py
                '''
                echo 'Python application compiled successfully!'
            }
        }

        stage('3. Build & Tag Docker Image') {
            steps {
                bat '''
                    docker build -t %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER% .
                    docker tag %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER% %REGISTRY%/%IMAGE_NAME%:latest
                '''
                echo 'Docker image built and tagged successfully!'
            }
        }

        stage('4. Push Image to Registry') {
            steps {
                bat '''
                    docker push %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER%
                    docker push %REGISTRY%/%IMAGE_NAME%:latest
                '''
                echo 'Docker images pushed successfully!'
            }
        }
    }

    post {
        success {
            echo '======================================'
            echo 'PIPELINE EXECUTED SUCCESSFULLY!'
            echo "Docker Image: ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
            echo '======================================'
        }

        failure {
            echo 'PIPELINE FAILED! Check Console Output.'
        }
    }
}
