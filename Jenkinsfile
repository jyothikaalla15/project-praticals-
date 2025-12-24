pipeline {
    agent any
    tools {
        nodejs 'node20' // This must match the name you gave it in Global Tool Configuration
    }

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

       stage('Verify Node') {
    steps {
        sh 'node -v'
        sh 'npm -v'
        sh 'ls -R' // This will show every file and folder in the workspace
    }
}

       stage('Build React') {
    steps {
        // Remove dir('frontend') if package.json is in the root
        sh 'npm install'
        sh 'npm run build'
    }
}

        stage('Deploy to S3 & Invalidate CloudFront') {
            steps {
                withAWS(credentials: 'aws-credentials', region: 'us-east-1') {
                    sh '''
                      aws s3 sync build/ s3://${S3_BUCKET} --delete
                      aws cloudfront create-invalidation \
                        --distribution-id E1QEK1LS9J2AK1 \
                        --paths "/*"
                    '''
                }
            }
        }
    }
}
