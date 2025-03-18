pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        GITHUB_TOKEN = credentials('github-token')
    }
    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/kgmimsdevops/DevOps-C-Lab.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-node-app .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                sh '''
                echo $DOCKERHUB_CREDENTIALS | docker login -u your-dockerhub-username --password-stdin
                docker tag my-node-app your-dockerhub-username/my-node-app:latest
                docker push your-dockerhub-username/my-node-app:latest
                '''
            }
        }
        stage('Deploy to EC2') {
            steps {
                sh '''
                docker run -d -p 8080:8080 --name my-container your-dockerhub-username/my-node-app:latest
                '''
            }
        }
        stage('Send Notification') {
            steps {
                sh '''
                aws sns publish --topic-arn arn:aws:sns:region:account-id:topic-name --message "Deployment Successful"
                '''
            }
        }
    }
}

