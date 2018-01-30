pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving ...'
                    archiveArtifacts artifacts: '**/*war'
                }
            }
        }
   
        stage('Deploy to staging') {
            steps {
                build job: 'first-maven_deploy_to_stage'
            }
        }

        stage('Deploy to production') {
            steps {
                timeout(time: 5, unit: 'DAYS'){
                    input message: 'Approve PRODUCTION Deployment?'
                }

                build job: 'first-mave_deploy_to_prod'
            }
            post {
                success {
                    echo 'Code deployed to Production ;)'
                }

                failure {
                    echo 'Deployment failed ;('
                }
            }
        }
    }
}

