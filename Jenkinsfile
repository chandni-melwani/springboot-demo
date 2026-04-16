pipeline {
    agent any

    environment {
        IMAGE_NAME = "chandni2308/demo-app:1.0"
        DOCKER_IMAGE = "chandni2308/demo-app"
        DOCKER_CREDENTIALS_ID = "dockerhub-creds"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/chandni-melwani/springboot-demo.git'
            }
        }

        stage('Build with Maven') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t demo-app .'
            }
        }

        stage('Tag Docker Image') {
            steps {
                bat 'docker tag demo-app %IMAGE_NAME%'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: DOCKER_CREDENTIALS_ID,
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push %IMAGE_NAME%'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat 'kubectl get pods'
                bat 'kubectl get services'
            }
        }
    }

    post {
        success {
            echo '✅ CI/CD Pipeline executed successfully!'
            echo '🚀 Application deployed to Kubernetes.'
        }
        failure {
            echo '❌ Pipeline failed. Check the console output for details.'
        }
        always {
            bat 'docker logout'
        }
    }
}