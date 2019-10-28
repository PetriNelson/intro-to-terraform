pipeline {
    agent {
        docker {
            image 'hashicorp/terraform:latest'
            args  '--entrypoint="" -u root -v /opt/jenkins/.aws:/root/.aws'
        }
    }
    parameters {
        choice(
            choices: ['preview' , 'apply' , 'show', 'preview-destroy' , 'destroy'],
            description: 'Terraform action to apply',
            name: 'action')
        string(defaultValue: "default", description: 'Which AWS Account (profile) do you want to target?', name: 'AWS_PROFILE')
    }
    stages {
        stage('init') {
            steps {
                 dir("${env.WORKSPACE}/s3-backend"){
                 sh "pwd"
                }
                sh 'terraform version'
            }
        }
        stage('validate') {
            when {
                expression { params.action == 'preview' || params.action == 'apply' || params.action == 'destroy' }
            }
            steps {
                sh 'terraform validate -var aws_profile=${AWS_PROFILE}'
            }
        }
        stage('preview') {
            when {
                expression { params.action == 'preview' }
            }
            steps {
                sh 'terraform plan -var aws_profile=${AWS_PROFILE}'
            }
        }
        stage('apply') {
            when {
                expression { params.action == 'apply' }
            }
            steps {
                sh 'terraform plan -out=plan -var aws_profile=${AWS_PROFILE}'
                sh 'terraform apply -auto-approve plan'
            }
        }
        stage('show') {
            when {
                expression { params.action == 'show' }
            }
            steps {
                sh 'terraform show'
            }
        }
        stage('preview-destroy') {
            when {
                expression { params.action == 'preview-destroy' }
            }
            steps {
                sh 'terraform plan -destroy -var aws_profile=${AWS_PROFILE}'
            }
        }
        stage('destroy') {
            when {
                expression { params.action == 'destroy' }
            }
            steps {
                sh 'terraform destroy -force -var aws_profile=${AWS_PROFILE}'
            }
        }
    }
}
