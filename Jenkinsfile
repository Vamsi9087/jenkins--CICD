pipeline {
    agent any

    environment {
        APP_NAME = "my-node-app"
        CONTAINER_NAME = "node-app"
        PORT = "3000"
    }

    stages {
        stage("Checkout") {
            steps {
                checkout scm
            }
        }

        stage("Build Docker Image") {
            steps {
                echo "🐳 Building Docker image..."
                sh "docker build -t ${APP_NAME}:latest ."
            }
        }

        stage("Deploy Container") {
            steps {
                echo "🚀 Deploying Docker container..."
                sh """
                if [ "$(docker ps -aq -f name=${CONTAINER_NAME})" ]; then
                    echo "🧹 Removing old container..."
                    docker rm -f ${CONTAINER_NAME}
                fi
                docker run -d -p ${PORT}:3000 --name ${CONTAINER_NAME} ${APP_NAME}:latest
                """
            }
        }

        stage("Show Logs") {
            steps {
                echo "📜 Showing container logs..."
                sh "sleep 3 && docker logs ${CONTAINER_NAME}"
            }
        }
    }

    post {
        success {
            echo "✅ App deployed successfully! Access it at: http://<your-server-ip>:${PORT}"
        }
        failure {
            echo "❌ Build or deployment failed."
        }
    }
}
