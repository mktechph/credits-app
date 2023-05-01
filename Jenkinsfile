pipeline {
    agent any

    environment {
        CI = 'true'
        AWS_REGION = 'ap-southeast-1'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Deliver for staging') {
            when {
                branch 'stg' 
            }
            steps {
                withAWS(region:$AWS_REGION, credentials:'AWS_S3_CREDITS-APP') {
                    s3Delete(bucket: 'credits-app-dev', path:'**/*')
                    s3Upload(bucket: 'credits-app-dev', workingDir:'build', includePathPattern:'**/*');
                }
            }
        }
        stage('Deploy for production') {
            when {
                branch 'main'  
            }
            steps {
                withAWS(region:$AWS_REGION, credentials:'AWS_S3_CREDITS-APP') {
                    s3Delete(bucket: 'credits-app-prod', path:'**/*')
                    s3Upload(bucket: 'credits-app-prod', workingDir:'build', includePathPattern:'**/*');
                }
            }
        }
    }
}