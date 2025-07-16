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
                dockerImage = docker.build("${DOCKER_REGISTRY}/${IMAGE_NAME}", "--platform=linux/amd64 .")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: '25dad33c-63be-43ed-97e2-31c54f7a9cf1',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                    )]) {
                        sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    }
            }
        }


        stage('Push Docker Image') {
            steps {
                script {
                    dockerImage.push()
                }
            }
        }


        stage('Run Docker container') {
            steps {
                script {
                    sh "docker rm -f ${IMAGE_NAME} || true"
                    docker.image("${DOCKER_REGISTRY}/${IMAGE_NAME}:latest").run("-d -p 3000:80 --name ${IMAGE_NAME}")
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
