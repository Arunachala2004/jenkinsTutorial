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
                // Use Windows commands instead of Unix ones
                bat '''
                if not exist deploy mkdir deploy
                xcopy /E /I /Y * deploy
                '''
            }
        }

        stage('Verify') {
            steps {
                echo '✅ Deployment complete! Files copied to deploy folder.'
                bat 'dir deploy'
            }
        }
    }
}
