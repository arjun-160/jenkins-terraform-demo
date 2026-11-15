pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
    }

    stages {
        stage('Checkout from Git') {
            steps {
                checkout scm
                sh 'ls -la'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init -input=false'
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -input=false -out=tfplan'
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -input=false -auto-approve tfplan'
            }
        }

        stage('Show Instance IP') {
            steps {
                sh '''
                    IP=$(terraform output -raw instance_public_ip)
                    echo "=========================================="
                    echo " EC2 INSTANCE CREATED SUCCESSFULLY"
                    echo " Public IP : $IP"
                    echo " Website   : http://$IP"
                    echo "=========================================="
                '''
            }
        }
    }

    post {
        success { echo 'Pipeline finished: instance is up (nginx needs ~1 minute to start).' }
        failure { echo 'Pipeline failed: read the red stage above for the error.' }
    }
}
