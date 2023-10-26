pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Authenticate with your Docker registry
                    sh 'docker login -u AWS -p $(aws ecr get-login-password --region env.REGION) env.ECR_URL'

                    // Build the Docker image
                    sh 'docker build -t env.ECR_URL/python-todo:env.BUILD_NUMBER .'

                    // Push the Docker image to ECR
                    sh 'docker push env.ECR_URL/python-tod:env.BUILD_NUMBER'
                }
            }
        }
    }
}
