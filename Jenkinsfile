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
                sh '''
                  echo "🧠 Checking environment..."
                  node -v
                  npm -v
                '''
            }
        }

        stage('🧹 Clean Workspace') {
            steps {
                sh '''
                  echo "🧹 Cleaning previous build artifacts..."
                  rm -rf node_modules package-lock.json dist
                '''
            }
        }

        stage('📦 Install Dependencies') {
            steps {
                sh '''
                  echo "📦 Installing npm dependencies..."
                  npm ci
                '''
            }
        }

        stage('🏗 Build Application') {
            steps {
                sh '''
                  echo "🏗 Building application..."
                  npm run build
                  echo "📁 Build output:"
                  ls -lh dist
                '''
            }
        }

        // stage('🚀 Deploy via CodeDeploy') {
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
