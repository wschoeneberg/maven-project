pipeline {
    agent any

    parameters {
        string(name: 'tomcat_stage', defaultValue: '10.11.12.13', description: 'Ip for Tomcat Stage')
        string(name: 'tomcat_prod',  defaultValue: '10.11.12.12', description: 'Ip for Tomcat Prod')
    }

    triggers {
        pollSCM('* * * * *')
    }

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

        stage('Deployments') {
            parallel {
                stage('Deploy to stage') {
                    steps {
                        sh "cp **/*war /opt/apache-tomcat/apache-tomcat-8.5.27-stage/webapps/ "
                    }
                }
 
                stage('Deploy to prod') {
                    steps {
                        sh "cp **/*war /opt/apache-tomcat/apache-tomcat-8.5.27-prod/webapps/ "
                    }

                }
            }
        } 
/*   
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
*/

    }
}

