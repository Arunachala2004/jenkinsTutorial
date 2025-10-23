pipeline {
    agent any

    environment {
        DEPLOY_FOLDER = 'deploy'
    }

    triggers {
        pollSCM('H/1 * * * *') // Poll GitHub every 1 minute
    }

    stages {
        stage('Checkout SCM') {
            steps {
                echo "📥 Checking out repository..."
                checkout scm
            }
        }

        stage('Prepare Deploy Folder') {
            steps {
                echo "🧹 Preparing deploy folder..."
                bat """
                if not exist %DEPLOY_FOLDER% mkdir %DEPLOY_FOLDER%
                """
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Deploying files using robocopy..."
                bat """
                REM robocopy returns non-zero codes even on success, normalize with exit 0
                robocopy . %DEPLOY_FOLDER% /MIR /XD %DEPLOY_FOLDER% /XF Jenkinsfile /NFL /NDL /NJH /NJS /NC /NS /NP || exit 0
                """
            }
        }

        stage('Verify') {
            steps {
                echo "🔍 Verifying deployment..."
                bat """
                if exist %DEPLOY_FOLDER% (
                    echo Deployment folder exists.
                    dir %DEPLOY_FOLDER%
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
            echo "✅ Pipeline completed successfully!"
            publishHTML([allowMissing: false,
                         alwaysLinkToLastBuild: true,
                         keepAll: true,
                         reportDir: DEPLOY_DIR,
                         reportFiles: 'index.html',
                         reportName: 'HTML Deployment'])
            
        }
        failure {
            echo "❌ Pipeline failed!"
        }
    }
}
