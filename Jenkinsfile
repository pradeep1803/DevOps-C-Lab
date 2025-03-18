pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'pradeep1803/my-node-app'
        AWS_REGION = 'us-east-1'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-creds', url: 'https://github.com/pradeep1803/DevOps-C-Lab'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB_REPO:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                    sh 'docker push $DOCKER_HUB_REPO:latest'
                }
            }
        }

        stage('Deploy with Terraform and Ansible') {
            steps {
                sh '''
                cd terraform
                terraform init
                terraform apply -auto-approve
                
                cd ../ansible
                ansible-playbook -i inventory deploy.yml
                '''
            }
        }

        stage('Notify on Success') {
            steps {
                sh 'aws sns publish --topic-arn arn:aws:sns:$AWS_REGION:123456789012:BuildNotifications --message "Deployment Successful"'
            }
        }
    }
}
