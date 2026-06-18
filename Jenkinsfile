pipeline {
    agent any 

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
                sh 'docker build -t jenkins-lab .'
            }
        }
        stage('Docker image') {
            steps {
                sh 'docker images | head'
            }
        }

    }
}