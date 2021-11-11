pipeline {
  environment {
    imagename = "shakurit0/my-first-docker-repo"
    registryCredential = 'dockerhub-cred'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: '*/*']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/valikkr/shared.git']]])

      }
    }
    stage('Building image') {
      steps{
        script {
          sh 'docker build -t shakurit0/my-first-docker-repo .'
        }
      }
    }
    stage('Pushing Image') {
      steps{
        script {
          docker.withRegistry( 'https://id.docker.com/login/?next=%2Fid%2Foauth%2Fauthorize%2F%3Fclient_id%3D43f17c5f-9ba4-4f13-853d-9d0074e349a7%26nonce%3DeyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiI0M2YxN2M1Zi05YmE0LTRmMTMtODUzZC05ZDAwNzRlMzQ5YTciLCJleHAiOiIyMDIxLTExLTExVDEwOjE2OjM1LjMyODMzMTE4NVoiLCJpYXQiOiIyMDIxLTExLTExVDEwOjExOjM1LjMyODMzMTY4OFoiLCJyZnAiOiJYdHROZ0JfQV9oUE85N2VQNEF5TU5nPT0iLCJ0YXJnZXRfbGlua191cmkiOiJodHRwczovL2h1Yi5kb2NrZXIuY29tIn0.omlh0BLg7S1aBYRxj--rtxyFQ8hJkNqaoP2dF0VfkpE%26redirect_uri%3Dhttps%253A%252F%252Fhub.docker.com%252Fsso%252Fcallback%26response_type%3Dcode%26scope%3Dopenid%26state%3DeyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiI0M2YxN2M1Zi05YmE0LTRmMTMtODUzZC05ZDAwNzRlMzQ5YTciLCJleHAiOiIyMDIxLTExLTExVDEwOjE2OjM1LjMyODMzMTE4NVoiLCJpYXQiOiIyMDIxLTExLTExVDEwOjExOjM1LjMyODMzMTY4OFoiLCJyZnAiOiJYdHROZ0JfQV9oUE85N2VQNEF5TU5nPT0iLCJ0YXJnZXRfbGlua191cmkiOiJodHRwczovL2h1Yi5kb2NrZXIuY29tIn0.omlh0BLg7S1aBYRxj--rtxyFQ8hJkNqaoP2dF0VfkpE', registryCredential ) {
           sh 'docker tag flask:latest shakurit0/my-first-docker-repo:${BUILD_NUMBER}'
            sh'docker push shakurit0/my-first-docker-repo:${BUILD_NUMBER}'
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"

      }
    }
  }
}
