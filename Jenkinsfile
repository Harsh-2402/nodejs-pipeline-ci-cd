pipeline {
    agent any

    tools {
        nodejs 'Node_22.11.0'
    }

    options {
        timestamps()                  // ⏱ Timestamped logs
     }

    environment {
        APP_NAME = 'nodejs-application'
        DEPLOY_GROUP = 'nodejs-application-DG'
        AWS_REGION = 'ap-south-1'
        S3_BUCKET = 'deploymasters-nodejs'
        BUILD_DIR = 'dist'
    }

     stages {

        stage('🔍 Environment Check') {
            steps {
                bat '''
                  echo 🧠 Checking environment...
                  node -v
                  npm -v
                '''
            }
        }

         // Linux
        // stage('🧹 Clean Workspace') {
        //     steps {
        //         bat '''
        //           echo 🧹 Cleaning old files...
        //           rmdir /s /q node_modules 2>NUL
        //           del package-lock.json 2>NUL
        //           rmdir /s /q dist 2>NUL
        //         '''
        //     }
        // }
         
         // Windows
         stage('🧹 Clean Workspace') {
            steps {
                bat '''
                  echo 🧹 Cleaning old files...
        
                  IF EXIST node_modules (
                    rmdir /s /q node_modules
                  )
        
                  IF EXIST package-lock.json (
                    del package-lock.json
                  )
        
                  IF EXIST dist (
                    rmdir /s /q dist
                  )
                '''
            }
        }


        stage('📦 Install Dependencies') {
            steps {
                bat '''
                  echo 📦 Installing dependencies...
                  npm ci
                '''
            }
        }

        stage('🏗 Build Application') {
            steps {
                bat '''
                  echo 🏗 Building app...
                  npm run build
                  dir dist
                '''
            }
        }

        // stage('🚀 Deploy to AWS CodeDeploy') {
        //     steps {
        //         echo "🚀 Deploying to AWS CodeDeploy..."
        //         step([
        //             $class: 'AWSCodeDeployPublisher',
        //             applicationName: "${APP_NAME}",
        //             deploymentGroupName: "${DEPLOY_GROUP}",
        //             region: "${AWS_REGION}",
        //             s3bucket: "${S3_BUCKET}",
        //             includes: "${BUILD_DIR}/",
        //             deploymentGroupAppspec: false,
        //             waitForCompletion: false
        //         ])
        //     }
        // }
    }

    post {
        success {
            echo "✅ Deployment completed successfully 🎉"
        }
        failure {
            echo "❌ Deployment failed 🚨 Check logs above"
        }
        always {
            echo "📌 Pipeline execution finished"
        }
    }
}
