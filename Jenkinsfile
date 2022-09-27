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
        post{
            always{
                echo "========always========"
            }
            success{
                echo "========pipeline executed successfully ========"
                slackSend channel: 'devopslearning', message: 'Your job runed Successfully'

    }
}