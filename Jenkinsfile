pipeline{
  agent any
	environment {
		VERSION = "${env.BUILD_ID}"
	}
  stages{
    stage('Maven Build & Sonar Quality Analysis'){
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
 stage('Docker Image Build'){
      steps{
	        script{
			withCredentials([string(credentialsId: 'nexus_password', variable: 'nexus-credentials')]) {
		         sh '''
			 docker build -t 13.126.213.157:8085/springapp:${VERSION} .	 
	                 '''
			}
		}
			
			
      }
 }
 stage('Push Docker Image to Nexus Server'){
      steps{
	        script{
			withCredentials([string(credentialsId: 'nexus_password', variable: 'nexus-credentials')]) {
		         sh '''		 
			 docker login -u admin -p admin123 13.126.213.157:8085
			 
			 docker push 13.126.213.157:8085/springapp:${VERSION}
			 
			 docker rmi 13.126.213.157:8085/springapp:${VERSION}
	                 '''
			}
		}
			
			
      }
 }

  
  
 }
}    
    
    
