pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo '📦 Cloning repository...'
                checkout scm
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying HTML files...'
                sh 'mkdir -p ~/jenkins-demo-deploy'
                sh 'cp *.html ~/jenkins-demo-deploy/'
            }
        }

        stage('Verify') {
            steps {
                echo '🔍 Checking deployment folder...'
                sh 'ls -l ~/jenkins-demo-deploy'
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful! Open ~/jenkins-demo-deploy/index.html to view it.'
        }
    }
}
