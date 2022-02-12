pipeline{
    agent any 
    stages{
        stage( "sonar quality check") {S
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar_token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'

                     
                     }
                }
            }
        }
    }
}
  
