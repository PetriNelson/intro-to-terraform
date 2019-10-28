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
                sh 'terraform version'
            }
        }
         stage('createS3Bucket') {
            steps {
                sh 'pwd'
            }
        }
    }
}
