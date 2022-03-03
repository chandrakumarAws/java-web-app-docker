node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/MithunTechnologiesDevOps/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
    } 
    stage('Build Docker Imager'){
   sh 'docker build -t chandrakumar420/javawebapp .'
   }
  stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'chandrakumar420', variable: 'dockerPassword')]) {
   sh "docker login -u chandrakumar420 -p ${dockerPassword}"
    }
   sh 'docker push chandrakumar420/javawebapp'
   }
   stage('Run Docker Image In Dev Server'){
   def dockerRun = ' docker run  -d -p 8080:8080 --name javawebapp chandrakumar420/javawebapp'
         sshagent(['Jenkinsubuntu']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.32.118 docker stop javawebapp || true'
          sh 'ssh  ubuntu@172.31.32.118 docker rm javawebapp  || true'
          sh 'ssh  ubuntu@172.31.32.118 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.32.118 ${dockerRun}"
       }
    }
}
