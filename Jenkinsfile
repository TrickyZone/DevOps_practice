pipeline{
    agent any
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
    }
}
