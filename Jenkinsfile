pipeline {
    agent any
    
    tools {
        terraform 'terraform'
    }
    
    stages {
        stage('Start deployment'){
            steps{
                echo 'Preparing environment...'
                git credentialsId: 'GitHub', url: 'https://github.globant.com/daniel-lalicata/apache-web-server.git'
                sh 'terraform init'
            }
        }
        
        stage('Plan deployment'){
            steps{
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'AWS', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                    sh 'terraform plan'
                }
            }
        }
        
        stage('Proceed or Abort'){
            input{
                message 'Would you like to deploy it on AWS?'
                ok 'Yes'
            }
            steps{
                echo 'Deploying...'
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'AWS', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                    sh 'terraform apply -auto-approve'
                }
            }
        }
        
        stage('Finish'){
            steps{
                echo 'Finish'
            }
        }
    }
}
