pipeline {
   environment {
    registry = 'dvquang/devops-cert'
    registryCredential = 'dvquang'
    image = ''
    deta = sh(script: "echo `date +'%Y%m%d'`", returnStdout: true).trim()
    dockerFile = 'Dockerfile'
   }
    
   agent any

   stages {
       stage('Clone source') {
           steps {
               git (url: 'https://github.com/dvquang10/devops.git', branch: 'master')
           }
       }
       stage('Build image') {
           steps {
               script {
                   //image = docker.build('$registry:$tagNum', '-f $dockerFile .')
                   image = docker.build(registry + ':$deta_$BUILD_NUMBER', '-f $dockerFile .')
               }
           }
          
       }
       stage('Push image') {
           steps {
               script {
                   //echo "Hiện tại đang bỏ qua bước này"
                    //withDockerRegistry('', registryCredential) {
                    withDockerRegistry(credentialsId: 'jenkins-dockerhub') {
                       image.push()
                    }
               }
           }
       }
       stage('Deploy frontend') {
           steps {
               echo "Deploy"
           }
       }
   }
}

