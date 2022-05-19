pipeline {
  environment {
    GOCACHE = "/tmp"
    imagename = "bee7ch/go-test"
    imagelocalname = "bee7ch-go-test"
    registryCredential = 'dockerhub_id'
    dockerImage = ''
  }
  agent any
  stages {
    //    stage('Build') {
    //        agent {
    //            docker {
    //                image 'golang'
    //            }
    //        }
    //        steps {
    //            // Create our project directory.
    //            sh 'cd ${GOPATH}/src'
    //            sh 'mkdir -p ${GOPATH}/src/hello-world'
    //            // Copy all files in our Jenkins workspace to our project directory.
    //            sh 'cp -r ${WORKSPACE}/* ${GOPATH}/src/hello-world'
    //            // Build the app.
    //            sh 'rm -f go.mod'
    //            sh 'go clean -cache'
    //            sh 'go mod init hello-world'
    //            sh 'go mod tidy'
    //            sh 'go build'
    //        }
    //    }
    //    stage('Test') {
    //        agent {
    //            docker {
    //                image 'golang'
    //            }
    //        }
    //        steps {
    //            // Create our project directory.
    //            sh 'cd ${GOPATH}/src'
    //            sh 'mkdir -p ${GOPATH}/src/hello-world'
    //            // Copy all files in our Jenkins workspace to our project directory.
    //            sh 'cp -r ${WORKSPACE}/* ${GOPATH}/src/hello-world'
    //            // Remove cached test results.
    //            sh 'rm -f go.mod'
    //            sh 'go clean -cache'
    //            sh 'go mod init hello-world'
    //            sh 'go mod tidy'
               
    //            // Run Unit Tests.
    //            sh 'go test ./... -v -short'
    //        }
    //    }

    // stage('Building image') {
    //   steps{
    //     script {
    //       dockerImage = docker.build imagename
    //     }
    //   }
    // }
    // stage('Push Image') {
    //   steps{
    //     script {
    //       docker.withRegistry( '', registryCredential ) {
    //         dockerImage.push("$BUILD_NUMBER")
    //         dockerImage.push('latest')

    //       }
    //     }
    //   }
    // }
    // stage('Remove Unused docker image') {
    //   steps{
    //      sh "docker rmi $imagename:$BUILD_NUMBER"
    //      sh "docker rmi $imagename:latest"

    //   }
    // }
    // stage('Run Docker image Localy') {
    //   steps{
    //     sh '''#!/bin/bash
    //     if [ "$(docker ps -q -f name=${imagelocalname})" ]
    //     then
    //      docker stop ${imagelocalname}
    //      docker rm ${imagelocalname} 2>/dev/null
    //      docker run --name=${imagelocalname} --rm -p 8081:8081 -d ${imagename}
    //     else
    //      docker run --name=${imagelocalname} --rm -p 8081:8081 -d ${imagename}
    //     fi
    //     '''
    //   }
    // }

    stage('Spin in kubernetes') {
           agent {
               docker {
                   image 'golang'
               }
           }
           steps {

                withCredentials([file(credentialsId: 'kube-config-microk8s', variable: 'KUBECONFIG')]) {
                   sh 'cd /tmp'
                   sh 'pwd'
                   sh 'curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl'
                   sh 'chmod +x ./kubectl'
                   sh 'mkdir -p /tmp/src/kubernetes'
                    // Copy all files in our Jenkins workspace to our project directory.
                   sh 'cp -r ${WORKSPACE}/* /tmp/src/kubernetes'
                   sh 'ls -la'
                   sh 'pwd'
                   sh '/tmp/kubectl version'
                   sh 'kubectl get pods -A'
                //    sh 'kubectl apply -f src/kubernetes/app-deployment.yaml'
                //    sh 'kubectl apply -f src/kubernetes/app-svc.yaml'
                }

           }
       }


  }
}