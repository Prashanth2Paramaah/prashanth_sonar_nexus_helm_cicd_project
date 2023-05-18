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
			 docker build -t 43.205.177.42:8085/springapp:${VERSION} .	 
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
			 docker login -u admin -p admin123 43.205.177.42:8085
			 
			 docker push 43.205.177.42:8085/springapp:${VERSION}
			 
			 docker rmi 43.205.177.42:8085/springapp:${VERSION}
	                 '''
			}
		}
			
			
      }
	 
 }
stage('Identifying misconfigs using datree in helm charts'){
	steps {
	    script {
	     dir('kubernetes/myapp/') {
                sh 'helm datree test .'
             }		
	   }
        }
		  
}
  
  
	
 }
post {
		always {
			mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", to: "itsprashanth327@gmail.com";  
		}
	}
}    
    
    
