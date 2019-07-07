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
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pass', usernameVariable: '')]) {                
                        sh "docker login https://index.docker.io/v1/ -p ${pass} -u jmch1692"
                        sh "docker build . -t jmch1692/timeoff:latest"
                        sh "docker push jmch1692/timeoff:latest"
                    }
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
