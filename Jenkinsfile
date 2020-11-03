pipeline {
   environment {
       registry = 'dvquang/devops-cert'             //   https://hub.docker.com/repository/docker/dvquang/devops-cert
       image = ''
       dockerFile = 'Dockerfile'                   //    Name of Dockerfile   
   }
    
   agent any

   stages {
      // Checkout source from Github, branch master
       stage('Checkout source') {
           steps {
               git (url: 'https://github.com/dvquang10/devops.git', branch: 'master')
               sh "git show -s --format=%B"
               echo "Current SHA1: `git rev-parse HEAD`"
           }
       }
      
      //  Build Docker image with Dockerfile
       stage('Build image') {
           steps {
               script {
                   image = docker.build(registry + ':$BUILD_NUMBER', '-f $dockerFile .')
               }
           }
          
       }
      
      //  Push image to Dockerhub
       stage('Push image to Dockerhub') {
           steps {
               script {
                       withDockerRegistry(credentialsId: 'jenkins-dockerhub') {    
                       image.push()
                    }
               }
           }
       }
      
      //  Remove image from Jenkins node
       stage('Remove image') {
           steps {
               sh "docker rmi $registry:$BUILD_NUMBER"
           }
       }
      
      //  Remove old container prepare for run new container
       stage('Remove old container') {
           steps {
               sh "docker stop webapp"
               sh "docker rm webapp"
           }
       }
      
      //  Run container use new image
       stage('Run container') {
           steps {
               sh "docker run -d -p 80:80 -e TZ=Asia/Ho_Chi_Minh --name=webapp ${registry}:${BUILD_NUMBER}"
           }
       }
   }
}
