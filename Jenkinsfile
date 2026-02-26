pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ArlagaddaHepseeba-git/static-website-cicd.git'
            }
        }
        stage('Deploy to S3') {
            steps {
                sh 'aws s3 sync . s3://new-hepsi-bucket --delete --exclude ".git/*" --exclude "Jenkinsfile"'
            }
        }
    }
    post {
        success {
            echo 'Website deployed successfully to S3!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
