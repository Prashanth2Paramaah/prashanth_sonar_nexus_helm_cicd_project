pipeline{
  agent any
	environment {
		VERSION = "${env.BUILD_ID}"
	}
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
 stage('docker build and docker push to nexus repository'){
      steps{
	        script{
			withCredentials([string(credentialsId: 'nexus_password', variable: 'nexus-credentials')]) {
		         sh '''
			 docker build -t 13.232.208.188:8033/springapp:${VERSION} .
			 
			 docker login -u admin -p admin123 13.232.208.188:8085
			 
			 docker push 13.232.208.188:8085/springapp:${VERSION}
			 
			 docker rmi 13.232.208.188:8085/springapp:${VERSION}
	                 '''
			}
		}
			
			
      }
 }

  
  
 }
}    
    
    
