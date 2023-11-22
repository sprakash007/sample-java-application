pipeline {
    agent any

    environment {
        // Define environment variables
        DOCKERHUB_CREDENTIALS = credentials('D-hub-cred') // ID of your Docker Hub credentials in Jenkins
        IMAGE_TAG = "prakash336/java-sample:latest" // Replace with your Docker Hub username and image name
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'dev', credentialsId: 'github-credentials', url: 'https://github.com/sprakash007/sample-java-application.git'

            }
        }

        stage('Build and Test') {
            steps {
                script {
                    // Run Maven build and test
                    sh 'mvn clean package'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Building Docker Image
                    sh "docker build -t ${IMAGE_TAG} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub
                    sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login --username ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                    // Pushing Image to Docker Hub
                    sh "docker push ${IMAGE_TAG}"
                }
            }
        }


     post {
         always {
            // Clean up Docker images
            sh "docker rmi ${IMAGE_TAG}"
         }
     }
  }
}

