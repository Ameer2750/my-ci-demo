pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-react-app'
        DOCKER_REGISTRY = 'vellaiameer'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Ameer2750/my-ci-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_REGISTRY}/${IMAGE_NAME}")
                }
            }
        }

        stage('Run container') {
            steps {
                script {
                    docker.image("${DOCKER_REGISTRY}/${IMAGE_NAME}").run('-p 3000:80')
                }
            }
        }

        
    }
}