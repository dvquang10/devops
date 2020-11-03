pipeline {
   environment {
    registry = 'dvquang/devops-cert'
    registryCredential = 'dvquang'
    image = ''
    //tagNum = sh(script: "echo `date +'%Y%m%d'`", returnStdout: true).trim()
    dockerFile = 'Dockerfile'
   }
    
   agent any

   stages {
       stage('Checkout source') {
           steps {
               git (url: 'https://github.com/dvquang10/devops.git', branch: 'master')
           }
       }
       stage('Build image') {
           steps {
               script {
                   //image = docker.build(registry + ':$tagNum', '-f $dockerFile .')
                   image = docker.build(registry + ':$BUILD_NUMBER', '-f $dockerFile .')
               }
           }
          
       }
       stage('Push image to Dockerhub') {
           steps {
               script {
                    withDockerRegistry(credentialsId: 'jenkins-dockerhub') {
                       image.push()
                    }
               }
           }
       }
       stage('Remove Image') {
           steps {
               sh "docker rmi $registry:$$BUILD_NUMBER"
           }
       }
   }
}

