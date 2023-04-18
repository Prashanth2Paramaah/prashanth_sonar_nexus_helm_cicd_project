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
      stage('docker build and docker push to nexus repository'){
          steps{
	      script{
				    
	      }
          }
      }
 }
}
    
    
    
