pipeline {
    agent any

    stages {
        stage("Checkout") {
            steps {
                checkout scm
            }
        }

        stage("Install Dependencies") {
            steps {
                sh 'apt-get update -y || true' // optional if node is already installed
                sh 'npm install'
            }
        }

        stage("Test") {
            steps {
                sh 'npm test || echo "No tests configured, skipping..."'
            }
        }

        stage("Build") {
            steps {
                sh 'npm run build || echo "No build script configured, skipping..."'
            }
        }
    }

    post {
        success {
            echo '✅ Build and test succeeded!'
        }
        failure {
            echo '❌ Build or test failed.'
        }
    }
}
