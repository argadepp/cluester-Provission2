pipeline {
    agent any

    environment {
        TF_VAR_aws_region = 'ap-south-1'
        
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/VishalBobade-Git/cluester-Provission.git'
            }
        }

        stage('Initialize Terraform') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: env.AWS_CREDENTIALS_ID]]) {
                    sh 'terraform init'
                }
            }
        }

        stage('Plan Infrastructure') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: env.AWS_CREDENTIALS_ID]]) {
                    sh 'terraform plan -out=tfplan'
                }
            }
        }

        stage('Apply Infrastructure') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: env.AWS_CREDENTIALS_ID]]) {
                    sh 'terraform apply -input=false tfplan'
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/*.tf', allowEmptyArchive: true
        }
    }
}
