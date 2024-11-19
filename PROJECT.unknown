pipeline {
    agent any

    environment {
        ECR_REPOSITORY = 'flaskapp'
        IMAGE_TAG = "${env.BUILD_ID}"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/naghamahmed/Sprints_W5-_Final', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${ECR_REPOSITORY}:${IMAGE_TAG}")
                }
            }
        }
        stage('Push to Nexus') {
            steps {
                script {
                    docker.withRegistry('http://nexus-service:8082', 'nexus-credentials') {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh 'kubectl apply -f k8s_manifests/'
            }
        }
    }
    post {
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
