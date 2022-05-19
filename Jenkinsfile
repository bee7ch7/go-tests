pipeline {
  environment {
    imagename = "bee7ch/go-test"
    imagelocalname = "bee7ch-go-test"
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
        if [ "$(docker ps -q -f name=${imagename})" ]
        then
         docker stop ${imagelocalname}
         docker rm ${imagelocalname} 2>/dev/null
         docker run --name=${imagelocalname} --rm -p8080:8080 -d ${imagename}
        else
         docker run --name=${imagelocalname} --rm -p8080:8080 -d ${imagename}
        fi
        '''
      }
    }
  }
}