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
			 docker build -t 52.66.86.136:8085/springapp:${VERSION} .	 
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
			 docker login -u admin -p admin123 52.66.86.136:8085
			 
			 docker push 52.66.86.136:8085/springapp:${VERSION}
			 
			 docker rmi 52.66.86.136:8085/springapp:${VERSION}
	                 '''
			}
		}
			
			
      }
	 
 }
stage('Identifying misconfigs using datree in helm charts'){
	steps {
	    script {
	     dir('kubernetes/myapp/') {
		  withEnv(['DATREE_TOKEN=13d538e9-1850-4be1-914f-35c12ea0bf3c']) {
                  sh 'helm datree test .'
		  }
             }		
	   }
        }
		  
}
stage('Pushing helm chart to the nexus repo') {
	steps{
		script{
		   withCredentials([string(credentialsId: 'nexus_password', variable: 'nexus-credentials')]) {
		      dir('kubernetes/myapp/') {
		          sh '''
			  helmversion=$(helm show chart . | grep version | cut -d: -f 2 | tr -d ' ')
			  tar -czvf myapp-${helmversion}.tgz myapp/
			  curl -u admin:$nexus_password http://52.66.86.136:8081/repository/helm-repo/ --upload-file myapp-${helmversion}.tgz -v
			  '''
		      }            
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
    
    
