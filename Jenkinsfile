pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'localhost', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost', description: 'Production Server')
    }

    tools {
        maven 'localMaven'
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "echo '' | sudo -S cp -rf **/target/*.war /Users/clakshma/Documents/Chethana/downloads/apache-tomcat-8.5.34-staging/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "echo '' | sudo -S cp -rf **/target/*.war /Users/clakshma/Documents/Chethana/downloads/apache-tomcat-8.5.34-prod/webapps"
                    }
                }
            }
        }
    }
}