pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'bhavya-bhavya'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/bhavyabandlam/bhavya_bhavya.git',
                    credentialsId: 'aws-credentials'
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
                withAWS(credentials: 'aws-credentials', region: 'us-east-1') {
                    sh """
                      aws s3 sync build/ s3://${S3_BUCKET} --delete
                      aws cloudfront create-invalidation \
                        --distribution-id E1OPEMDLPY5RD6 \
                        --paths "/*"
                    """
                }
            }
        }
    }
}

