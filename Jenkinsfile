pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/opt/homebrew/bin:/usr/bin:/bin:/usr/sbin:/sbin"
        AWS_DEFAULT_REGION = "us-east-1"
        S3_BUCKET = "amzn-nextgen"
        CLOUDFRONT_DIST_ID = "E1QEK1LS9J2AK1"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/jyothikaalla15/project-praticals-.git'
            }
        }

        stage('Verify Node') {
            steps {
                sh '''
                    echo "PATH=$PATH"
                    which node
                    node -v
                    which npm
                    npm -v
                '''
            }
        }

        stage('Build React') {
            steps {
                sh '''
                    npm install
                    npm run build
                '''
            }
        }

        stage('Deploy to S3 & Invalidate CloudFront') {
            steps {
                sh '''
                    aws s3 sync build/ s3://$S3_BUCKET --delete
                    aws cloudfront create-invalidation \
                      --distribution-id $CLOUDFRONT_DIST_ID \
                      --paths "/*"
                '''
            }
        }
    }
}
