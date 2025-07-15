pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-react-app'
        DOCKER_REGISTRY = 'vellaiameer' // Use this if pushing to Docker Hub later
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
                    dockerImage = docker.build("${DOCKER_REGISTRY}/${IMAGE_NAME}")
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Stop existing container if it exists
                    sh "docker rm -f ${IMAGE_NAME} || true"

                    // Run the new container
                    dockerImage.run("-d -p 3000:80 --name ${IMAGE_NAME}")
                }
            }
        }
    }

    post {
        failure {
            echo "Build failed ❌"
        }
        success {
            echo "Build & container run successful ✅"
        }
    }
}
