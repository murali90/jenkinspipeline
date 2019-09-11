pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '13.233.85.78', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '13.233.114.195', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
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
                        sh "scp -i /c/Users/nagamurallidhar/Desktop/keypairs/jenkinsserver.pem **/target/*.war ec2-user@${params.tomcat_dev}:/opt/tomcat-8.5.45/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh ""scp -i /c/Users/nagamurallidhar/Desktop/keypairs/jenkinsserver.pem **/target/*.war ec2-user@${params.tomcat_prod}:/opt/tomcat-8.5.45/webapps"
                    }
                }
            }
        }
    }
}
