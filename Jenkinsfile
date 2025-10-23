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
                bat """
                REM robocopy returns non-zero codes even on success, normalize with exit 0
                robocopy . %DEPLOY_DIR% /MIR /XD %DEPLOY_DIR% /XF Jenkinsfile /NFL /NDL /NJH /NJS /NC /NS /NP || exit 0
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
            // Publish HTML report
            publishHTML([allowMissing: false,
                         alwaysLinkToLastBuild: true,
                         keepAll: true,
                         reportDir: DEPLOY_DIR,
                         reportFiles: 'index.html',
                         reportName: 'HTML Deployment'])
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
