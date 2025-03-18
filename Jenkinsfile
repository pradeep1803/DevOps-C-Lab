pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        GITHUB_TOKEN = credentials('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')
    }
    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/pradeep1803/DevOps-C-Lab'
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
                docker tag my-node-app pradeep1803/my-node-app:latest
                docker push pradeep1803/my-node-app:latest
                '''
            }
        }
        stage('Deploy to EC2') {
            steps {
                sh '''
                docker run -d -p 8080:8080 --name my-container pradeep1803/my-node-app:latest
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

