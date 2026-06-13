pipeline {
    agent any
   
    stages {

        stage('Prepare directory') {
            steps{
                sh 'rm -rf /var/jenkins_home/tomcat-web || true'
                sh 'mkdir -p /var/jenkins_home/tomcat-web'
            }
        }

        stage('Copy app to Jenkins') {
            steps {
                sh '''
                echo "Verificando carpeta shopping:"
                ls -la
                ls -la shopping

                mkdir -p /var/jenkins_home/tomcat-web/shopping
                cp -r shopping/* /var/jenkins_home/tomcat-web/shopping
                '''
            }
        }

        stage('Create Tomcat container') {
            steps {
                sh '''
                docker rm -f tomcat1 || true
#VERSION DE TOMCAT QUE NO EXISTE A PROPOSITO
                docker run -dit --name tomcat1 -p 9090:8080 tomcat:19.0
                '''
            }
        }

        stage('Deploy app to Tomcat') {
            steps {
                sh '''
                echo "Copiando app al contenedor Tomcat..."
                docker cp /var/jenkins_home/tomcat-web/shopping tomcat1:/usr/local/tomcat/webapps/
                '''
            }
        }

        stage('Verify deployment') {
            steps {
                sh '''
                echo "Verificando dentro del contenedor:"
                docker exec tomcat1 ls /usr/local/tomcat/webapps/
                '''
            }
        }
    }

    post {
        always {
            echo 'These steps are always executed'   
            cleanWs()         
        }
      
        success {
        // One or more steps need to be included within each condition's block.
          echo 'the deployment has worked'
                
       }
       failure {
        // One or more steps need to be included within each condition's block.
        echo 'An error has ocurred'
      }
         }
    }
