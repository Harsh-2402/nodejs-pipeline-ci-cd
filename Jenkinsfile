pipeline {
    agent any

    tools {
        nodejs 'Node_22.11.0'
    }

    options {
        timestamps()
    }

    environment {
        APP_NAME    = 'nodejs-application'
        DEPLOY_GROUP = 'nodejs-application-DG'
        AWS_REGION  = 'ap-south-1'
        S3_BUCKET   = 'deploymasters-nodejs'
        BUILD_DIR   = 'dist'
    }

    stages {

        stage('Environment Check') {
            steps {
                script {
                    runCmd("""
                        echo Checking environment...
                        node -v
                        npm -v
                    """)
                }
            }
        }

        stage('Clean Workspace') {
            steps {
                script {
                    runCmd("""
                        echo Cleaning old files...

                        if [ -d node_modules ]; then rm -rf node_modules; fi
                        if [ -f package-lock.json ]; then rm -f package-lock.json; fi
                        if [ -d dist ]; then rm -rf dist; fi
                    """, """
                        echo Cleaning old files...

                        IF EXIST node_modules rmdir /s /q node_modules
                        IF EXIST package-lock.json del package-lock.json
                        IF EXIST dist rmdir /s /q dist
                    """)
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    runCmd("""
                        echo Installing dependencies...
                        npm install
                    """)
                }
            }
        }

        stage('Build Application') {
            steps {
                script {
                    runCmd("""
                        echo Building app...
                        npm run build
                        ls -l dist
                    """, """
                        echo Building app...
                        
                    """)
                }
            }
        }

        // Optional AWS CodeDeploy stage (works on both)
        /*
        stage('Deploy to AWS CodeDeploy') {
            steps {
                echo "Deploying to AWS CodeDeploy..."
                step([
                    $class: 'AWSCodeDeployPublisher',
                    applicationName: "${APP_NAME}",
                    deploymentGroupName: "${DEPLOY_GROUP}",
                    region: "${AWS_REGION}",
                    s3bucket: "${S3_BUCKET}",
                    includes: "${BUILD_DIR}/",
                    deploymentGroupAppspec: false,
                    waitForCompletion: false
                ])
            }
        }
        */
    }

    post {
        success {
            echo "[OK] Deployment completed successfully"
        }
        failure {
            echo "[ERROR] Deployment failed – check logs above"
        }
        always {
            echo "(*) Pipeline execution finished"
        }
    }
}

/**
 * Helper method to run OS-specific commands
 */
def runCmd(String linuxCmd, String windowsCmd = null) {
    if (isUnix()) {
        sh linuxCmd
    } else {
        bat windowsCmd ?: linuxCmd
    }
}
