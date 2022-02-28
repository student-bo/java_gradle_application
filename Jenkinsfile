pipeline{
    agent any 
    environment{
        VERSION = "${env.BUILD_ID}"
    }
    stages{
        stage( "sonar quality check") {
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'Sonar-Token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
                     }

                     timeout(time: 1, unit: 'HOURS') {
                         def qg = waitForQualityGate()
                         if (qg.status != 'OK') {
                             error "pipeline aborted due to quality gate failure: ${qg.status}"
                         }
                     }
                }
            }
        }
        stage("Docker Build & Docker Push"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker_passwd', variable: 'docker-passwd')]) {
                         sh '''
                            docker build -t 54.146.228.180:8083/springapp:${VERSION} .
                            docker login -u admin -p $docker-passwd 54.146.228.180:8083
                            docker push 54.146.228.180:8083/springapp:${VERSION}
                            docker rmi 54.146.228.180:8083/springapp:${VERSION}
                        '''

                    }
                   
                }
            }
        }
    }
}
  
