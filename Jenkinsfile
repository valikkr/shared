pipeline {
    agent any
    triggers { pollSCM('* * * * *') }
    options {
        buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5'))
        timestamps()
    }
    environment {
        //TODO # 1 --> once you sign up for Docker hub, use that user_id here
        registry = "shakurit0/my-first-docker-repo:${BUILD_NUMBER}"
        registry_1 = "shakurit0/my-first-docker-repo:latest"
        //TODO #2 - update your credentials ID after creating credentials for connecting to Docker Hub
        registryCredential = 'dockerhub-cred'
        dockerImage = 'shakurit0/my-first-docker-repo:latest'
    }
    
    stages {
        stage('Cloning Git') {
            steps {
                // make link via Pipeline Syntax
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/valikkr/shared']]])       
            }
        }
    
    // Building Docker images
    stage('Building image') {
      steps{
        script {
            dockerImage = docker.build registry
            dockerImage_1 = docker.build registry_1
        }
      }
    }
    
     // Uploading Docker images into Docker Hub
    stage('Upload Image') {
     steps{    
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
            }
        }
      }
    }
    
     // Stopping Docker containers for cleaner Docker run
     stage('docker stop container') {
         steps {
            sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
            sh ' docker rmi $registry'
         }
       }
    
    
    // Running Docker container, make sure port 8096 is opened in 
    stage('Docker Run') {
     steps{
         script {
            dockerImage_1.run("-p 5000:5000 --rm --name mypythonContainer")
         }
      }
    }
  }
}
