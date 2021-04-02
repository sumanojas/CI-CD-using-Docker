pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }
    stages {
      stage('checkout') {
           steps {
             git branch: 'master', url: 'https://github.com/sumanojas/CI-CD-using-Docker.git'
             }
        }
      stage('Execute Maven') {
           steps {
             def mvnHome = tool name: 'maven-3', type: 'maven'
             def mvnCMD = "${mvnHome}/bin/mvn"
             sh "${mvnCMD} clean package"            
          }
        }
      stage('Docker Build and Tag') {
           steps {
             sh 'docker build -t sumanojas:latest .' 
             sh 'docker tag samplewebapp sumanojas/samplewebapp:latest'
           //sh 'docker tag samplewebapp sumanojas/samplewebapp:$BUILD_NUMBER'
          }
        }
     
      stage('Publish image to Docker Hub') {
           steps {
             withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
             sh  'docker push sumanojas/samplewebapp:latest'
         //  sh  'docker push sumanojas/samplewebapp:$BUILD_NUMBER' 
        }
        }
        }
     
      stage('Run Docker container on Jenkins Agent') {
	   steps {
             sh "docker run -d -p 8090:8080 sumanojas/samplewebapp"
  
            }
        }
  
    }
	}
    
