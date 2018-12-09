pipeline {
    agent any
    
    parameters { 
         //string(name: 'tomcat_dev', defaultValue: '34.201.60.56', description: 'Staging Server')
         //string(name: 'tomcat_prod', defaultValue: '54.161.234.137', description: 'Production Server')
         string(name: 'tomcat_dev_local', defaultValue: 'localhost:8090', description: 'Staging Server')
         string(name: 'tomcat_prod_local', defaultValue: 'localhost:9090', description: 'Production Server')
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
                        //AMAZON EC2
                        sh "scp -i /Users/clemente.sanchez/Documents/DevTools/aws/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                        //Localhost 8090 
                        //sh "cp  **/target/*.war /Users/clemente.sanchez/Documents/DevTools/apache-tomcat-9.0.13-staging/webapps/"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                         //AMAZON EC2
                        sh "scp -i /Users/clemente.sanchez/Documents/DevTools/aws/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                        //Localhost 9090
                        sh "cp  **/target/*.war /Users/clemente.sanchez/Documents/DevTools/apache-tomcat-9.0.13-prd/webapps/"

                    }
                }
            }
        }
    }
}