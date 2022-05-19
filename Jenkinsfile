pipeline {
  environment {
    imagename = "bee7ch/gotest"
    registryCredential = 'dockerhub_id'
    dockerImage = ''
  }
  agent any
  stages {
    // stage('Cloning Git') {
    //   steps {
    //     git([url: 'https://bee7ch_bitbucket@bitbucket.org/bee7ch_bitbucket/lesson3.git', branch: 'master'])

    //   }
    // }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
            dockerImage.push('latest')

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
    stage('Run Docker image') {
      steps{
        sh '''#!/bin/bash
        if [ "$(docker ps -q -f name=bee7ch/gotest)" ]
        then
         docker stop bee7ch/gotest
         docker rm bee7ch/gotest 2>/dev/null
         docker run --name=bee7ch/gotest --rm -p8080:8080 -d bee7ch/gotest
        else
         docker run --name=bee7ch/gotest --rm -p8080:8080 -d bee7ch/gotest
        fi
        '''
      }
    }
  }
}