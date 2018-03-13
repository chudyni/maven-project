pipeline {
    agent any

    parameters {
        string(name: 'tomcat_stage', defaultValue: '18.221.237.27', description: 'Staging')
        string(name: 'tomcat_prod', defaultValue: '18.219.222.48', description: 'PROD')
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
                    echo 'Building war...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        sh "scp /var/lib/jenkins/workspace/FullyAutomated/webapp/target/webapp.war ec2-user@${params.tomcat_stage}:/var/lib/tomcat7/webapps"

                    }
                }

                stage('Deploy to PROD') {
                    steps {
                        sh "scp /var/lib/jenkins/workspace/FullyAutomated/webapp/target/webapp.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}