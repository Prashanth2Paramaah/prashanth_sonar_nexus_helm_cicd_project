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
			 docker build -t 13.127.196.78/springapp:${VERSION} .
			 
			 docker login -u admin -p admin123 13.232.208.188:8083
			 
			 docker push 13.232.208.188/springapp:${VERSION}
			 
			 docker rmi 13.232.208.188/springapp:${VERSION}
	                 '''
			}
		}
			
			
      }
 }

  
  
 }
}    
    
    
