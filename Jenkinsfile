pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'demo22112025'
    }

    stages {
        stage('Checkout') {
            steps {
                // Use GitHub credentials here (NOT aws-credentials)
                git branch: 'main',
                    url: 'https://github.com/Amruta9993/Frontend-new-project.git',
                    credentialsId: 'jenkins-credentials'   // <-- FIXED
            }
        }

        stage('Build React') {
            steps {
                sh 'chmod +x build.sh'
                sh './build.sh'
            }
        }

        stage('Deploy to S3 & Invalidate CloudFront') {
            steps {
                // Use AWS credentials only for AWS operations
                withAWS(credentials: 'aws-credentials', region: 'us-east-1') {
                    sh 'aws s3 sync build/ s3://$S3_BUCKET --delete'
                    sh 'aws cloudfront create-invalidation --distribution-id E37X9XTDU08S9W --paths "/*"'
                }
            }
        }
    }
}
