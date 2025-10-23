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
                echo '🚀 Deploying HTML files...'
                // Copy all files/folders except the deploy folder itself
                bat """
                for %%F in (*) do (
                    if /I not "%%F"=="%DEPLOY_DIR%" xcopy "%%F" "%DEPLOY_DIR%" /E /I /Y
                )
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
