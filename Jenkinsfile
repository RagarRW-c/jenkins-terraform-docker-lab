pipeline {
    agent any 

    environment {
        ECR_REPO = '613136968754.dkr.ecr.eu-central-1.amazonaws.com/jenkins-lab'
    }

    stages {

        stage('Check Tools') {
            steps {
                sh 'git --version'
                sh 'docker --version'
                sh 'terraform --version'
                sh 'aws --version'

            }
        }

        stage('Terraform format') {
            steps {
                sh 'terraform fmt -check'
            }
        }

        stage('Terraform init') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Terraform validate') {
            steps {
                sh 'terraform validate'
            }
        }
        stage('AWS Identity') {
            steps {
                sh 'aws sts get-caller-identity'
            }
        }
        stage('Docker build') {
            steps {
                sh '''
                    mkdir -p "$WORKSPACE/.docker"
                    echo '{"auths":{}}' > "$WORKSPACE/.docker/config.json"

                    DOCKER_CONFIG="$WORKSPACE/.docker" docker build -t $ECR_REPO:latest .
                '''
                }
        }

        stage ('ECR login') {
            steps {
                sh '''
                    mkdir -p "$WORKSPACE/.docker"
                    aws ecr get-login-password --region eu-central-1 | DOCKER_CONFIG="$WORKSPACE/.docker" docker login --username AWS --password-stdin $ECR_REPO
                    '''
            }
        }

        stage ('Push to ECR') {
            steps {
                sh '''
                    DOCKER_CONFIG="$WORKSPACE/.docker" docker push $ECR_REPO:latest
                    '''
            }
        }
        stage('Docker image') {
            steps {
                sh 'docker images | head'
            }
        }

    }
}