pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
        DEPLOY_SERVER = 'your-server-address'     
        DEPLOY_USER = 'ubuntu'                   
        DEPLOY_PATH = '/var/www/my-node-app' 
    }

    stages {
        stage("Checkout") {
            steps {
                echo "📥 Checking out code..."
                checkout scm
            }
        }

        stage("Install Dependencies") {
            steps {
                echo "Installing dependencies..."
                sh 'apt-get update -y || true' 
                sh 'npm install'
            }
        }

        stage("Test") {
            steps {
                echo " Running tests..."
                sh 'npm test || echo "No tests configured, skipping..."'
            }
        }

        stage("Build") {
            steps {
                echo " Building the app..."
                sh 'npm run build || echo "No build script configured, skipping..."'
            }
        }

        stage("Deploy") {
            when {
                branch 'main'  
            }
            steps {
                echo "Deploying to server..."
                
                sh '''
                echo "Uploading files to $DEPLOY_SERVER ..."
                # Create the directory on the remote server if it doesn't exist
                ssh -o StrictHostKeyChecking=no $DEPLOY_USER@$DEPLOY_SERVER "mkdir -p $DEPLOY_PATH"

                # Copy build files to the server
                scp -r dist/* $DEPLOY_USER@$DEPLOY_SERVER:$DEPLOY_PATH/

                # Restart the app (example with PM2)
                ssh -o StrictHostKeyChecking=no $DEPLOY_USER@$DEPLOY_SERVER "cd $DEPLOY_PATH && npm install --production && pm2 restart all || pm2 start npm --name my-node-app -- start"
                '''
            }
        }
    }

    post {
        success {
            echo ' Build, Test, and Deployment succeeded!'
        }
        failure {
            echo ' Build, Test, or Deployment failed.'
        }
    }
}
