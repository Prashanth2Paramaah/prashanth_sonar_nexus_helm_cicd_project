pipeline{
  agent any
  stages{
    stage('sonar quality status'){
      agent{
        docker {
          image 'maven'
          args '-u root'
        }
      }
      steps{
        script{
          withSonarQubeEnv(credentialsId: 'sonar-token') {
              sh 'mvn clean package sonar:sonar'
          }
        }
      }
   }
    stage('Quality Gate'){
      steps{
        script{
          waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
        }
      }
     
    }
 }
}
    
    
    
