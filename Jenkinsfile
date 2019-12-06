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
            agent 'dev-slave'
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
            agent 'dev-slave'
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
    echo 'building'
}

void mvDeploy(){
    echo 'deploying...'
}
