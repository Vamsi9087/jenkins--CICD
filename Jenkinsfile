pipeline {
    agent any 
    stages {
        stage("Checkout"){
            steps {
                checkout scm
            }
        }
        stage("Test") {
            steps {
                sh '''
                    set -e
                    sudo apt-get update
                    sudo apt-get install -y npm
                    npm test
                '''
            }
        }
        stage("Build"){
            steps{
                sh 'npm run build'
            }
        }
    }
}
