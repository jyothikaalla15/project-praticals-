pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'amzn-nextgen'
        CLOUDFRONT_DISTRIBUTION = 'E1QEK1LS9J2AK1'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/jyothikaalla15/project-praticals-.git,
                    credentialsId: 'aws-credentials'
            }
        }

        stage('Install & Build React') {
            steps {
                sh '''
                    npm install
                    npm run build
                '''
            }
        }

        stage('Deploy to S3') {
            steps {
                withAWS(credentials: 'aws-credentials', region: AWS_DEFAULT_REGION) {
                    sh 'aws s3 sync build/ s3://$S3_BUCKET --delete'
                }
            }
        }

        stage('Invalidate CloudFront') {
            steps {
                withAWS(credentials: 'aws-credentials', region: AWS_DEFAULT_REGION) {
                    sh 'aws cloudfront create-invalidation --distribution-id $CLOUDFRONT_DISTRIBUTION --paths "/*"'
                }
            }
        }
    }
}
