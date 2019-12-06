pipeline {
    agent none
    options {
        timestamps()
    }

    stages(){
        stage('CI - Pull request'){
            when {
                changeRequest target: 'develop'
                beforeAgent true
            }
            agent { label 'dev-slave' }
            steps{
                script{
                    ciBuild('develop')
                }
            }
        }

        stage('CI/CD develop'){
            when {
                branch 'develop'
                beforeAgent true
            }
            agent { label 'dev-slave' }
            steps{
                script{
                    ciBuild('develop')
                    mvDeploy()
                }
            }
        }
    }
}

void ciBuild(String target_env) {
    docker.withRegistry("registry.hub.docker.com/library") {
            docker.image("node") {
                sh "npm -v"
            }
        }
}

void mvDeploy(){
    echo 'deploying...'
}
