pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '52.87.187.59', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.90.9.87', description: 'Production Server')
    } 
 
    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }
 
stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
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
                        bat  "C:/Program Files/Git/git-bash.exe" --cd-to-home "scp -i /demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
 
                stage ('Deploy to Production'){
                    steps {
                        bat "WinSCP -i C:/Users/Divya/demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
