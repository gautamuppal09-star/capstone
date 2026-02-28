pipeline {
    agent any

    environment {
        AWS_APP_IP = '13.220.182.104'
        AZURE_APP_IP = '20.124.112.221'
        SSH_KEY = '/home/ubuntu/22oct.pem'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/gautamuppal09-star/capstone.git'
            }
        }

        stage('Deploy to AWS') {
            steps {
                sh '''
                    scp -i ${SSH_KEY} -o StrictHostKeyChecking=no index-aws.html ubuntu@${AWS_APP_IP}:/var/www/html/index.nginx-debian.html
                    ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ubuntu@${AWS_APP_IP} "sudo systemctl restart nginx"
                '''
            }
        }

        stage('Deploy to Azure') {
            steps {
                sh '''
                    scp -i ${SSH_KEY} -o StrictHostKeyChecking=no index-azure.html azureuser@${AZURE_APP_IP}:/var/www/html/index.nginx-debian.html
                    ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no azureuser@${AZURE_APP_IP} "sudo systemctl restart nginx"

                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
