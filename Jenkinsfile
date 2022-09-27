pipeline{
    agent any
    environment {
        VERSION = "${env.BUILD_ID}"
    }
    stages{
        
        stage("Sonar Quality Checks"){
            // agent {
            //     docker {
            //         image "openjdk:11"
            //     }
            // }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'Sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
           
                    }
                
                }
            }
        }


        stage("Docker build and docker push"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'Nexuspassfordocker', variable: 'nexus')]) {

                        sh """
                            docker build -t 3.6.38.239:8083/springboot:${VERSION} .
                            docker login -u admin -p $nexus 3.6.38.239:8083 
                            docker push 3.6.38.239:8083/springboot:${VERSION}
                            docker rmi 3.6.38.239:8083/springboot:${VERSION}
                        """
   
                    }
                    
                }
            }
            
        }
        post {
		always {
            slackSend channel: 'devopslearning', message: 'Your job had been started'

			// mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", to: "deekshith.snsep@gmail.com";  
		}
	}
    }
}