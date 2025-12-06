pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "durga83125/health-centre"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/Durgarao101/realtime-project.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker Image: ${DOCKER_HUB_REPO}:${IMAGE_TAG}"
                    docker.build("${DOCKER_HUB_REPO}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    echo "Pushing image to Docker Hub..."
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        def img = docker.image("${DOCKER_HUB_REPO}:${IMAGE_TAG}")
                        img.push()
                        img.push("latest")
                    }
                }
            }
        }
    }

    post {
        always {
            sh 'docker system prune -f || true'
        }
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed. Please check logs."
        }
    }
}
