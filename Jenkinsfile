pipeline{
  agent any
	environment {
		VERSION = "${env.BUILD_ID}"
	}
  stages{
    stage('Maven Build & Sonar Quality Status'){
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
 stage('Docker Build'){
      steps{
	        script{
			withCredentials([string(credentialsId: 'nexus_password', variable: 'nexus-credentials')]) {
		         sh '''
			 docker build -t 3.6.38.94:8085/springapp:${VERSION} .	 
	                 '''
			}
		}
			
			
      }
 }
 stage('Push Image to Nexus Server'){
      steps{
	        script{
			withCredentials([string(credentialsId: 'nexus_password', variable: 'nexus-credentials')]) {
		         sh '''		 
			 docker login -u admin -p admin123 3.6.38.94:8085
			 
			 docker push 3.6.38.94:8085/springapp:${VERSION}
			 
			 docker rmi 3.6.38.94:8085/springapp:${VERSION}
	                 '''
			}
		}
			
			
      }
 }

  
  
 }
}    
    
    
