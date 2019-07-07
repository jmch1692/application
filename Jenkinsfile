pipeline{
    agent any
    stages{
        stage('Checkout'){
            steps{
                git 'https://github.com/jmch1692/application.git'
            }
        }
        stage('Install dependencies'){
            steps{
                sh "npm install"
            }
        }
        /*stage('Test'){
            steps{
                sh "npm test"
            }
        }*/
        stage('Build and Push Docker Image'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'azure-registry', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh "docker login myregistryjmch.azurecr.io -u ${user} -p ${pass}"
                    sh "docker build . -t myregistryjmch.azurecr.io/timeoff:${BUILD_NUMBER}"
                    sh "docker push myregistryjmch.azurecr.io/timeoff:${BUILD_NUMBER}"
                }
        }
        stage('Restart web site'){
              steps{
                azureCLI commands: [[exportVariablesString: '', script: 'az webapp restart --name timeoff --resource-group gorilla-demo']], principalCredentialId: 'jenkins-azure'   
              }
        }
    }
    post{
        always{
            cleanWs()
        }
    }
    
}
