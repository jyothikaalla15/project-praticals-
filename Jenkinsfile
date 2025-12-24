pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'amzn-nextgen'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/jyothikaalla15/project-praticals-.git'
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
                      sh '''
                      /usr/local/bin/aws --version
                      /usr/local/bin/aws s3 sync build/ s3://amzn-nextgen --delete'
                       aws cloudfront create-invalidation \
                        --distribution-id E1QEK1LS9J2AK1 \
                        --paths "/*"
                    '''
                }
            }
        }
    }
}
