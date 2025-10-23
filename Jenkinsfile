pipeline {
    agent any

    environment {
        DEPLOY_DIR = 'deploy'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Prepare Deploy Folder') {
            steps {
                echo '🗂️ Preparing deploy folder...'
                bat """
                if not exist %DEPLOY_DIR% mkdir %DEPLOY_DIR%
                """
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying files using robocopy...'
                // /MIR mirrors the source to destination, /XD excludes directories, /XF excludes files
                bat """
                robocopy . %DEPLOY_DIR% /MIR /XD %DEPLOY_DIR% /XF Jenkinsfile /NFL /NDL /NJH /NJS /nc /ns /np
                """
            }
        }

        stage('Verify') {
            steps {
                echo '🔍 Verifying deployment...'
                bat """
                if exist %DEPLOY_DIR% (
                    echo Deployment folder exists.
                    dir %DEPLOY_DIR%
                ) else (
                    echo Deployment failed!
                    exit /b 1
                )
                """
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
