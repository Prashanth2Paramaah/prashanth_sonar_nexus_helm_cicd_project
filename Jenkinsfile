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
			 docker build -t 13.127.196.78/paramaahtestapp:${VERSION} .
			 
			 docker login -u admin -p admin123 13.127.196.78:8083
			 
			 docker push 13.127.196.78/paramaahtestapp:${VERSION}
			 
			 docker rmi 13.127.196.78/paramaahtestapp:${VERSION}
	                 '''
			}
		}
			
			
      }
 }

  
  
 }
}    
    
    
