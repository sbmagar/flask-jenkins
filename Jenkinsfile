pipeline {
    // agent any
    agent {
        docker { image 'python:3.8' }
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'main', url: 'https://github.com/sbmagar/jenkins-docker-flask.git'
             
          }
        }
  stage('Execute PIP Installation') {
           steps {
             
                sh 'pip install -r requirements.txt'             
          }
        }
stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t flask_app:latest .' 
                sh 'docker tag flask_app sbmagar/flask_app:latest'
                //sh 'docker tag flask_app sbmagar/flask_app:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
          sh  'docker push sbmagar/flask_app:latest'
        //  sh  'docker push sbmagar/flask_app:$BUILD_NUMBER' 
        }
                  
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
   {
                sh "docker run -d -p 5000:5000 sbmagar/flask_app"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://jenkins@172.31.28.25 run -d -p 5000:5000 sbmagar/flask_app"
 
            }
        }
    }
 }
