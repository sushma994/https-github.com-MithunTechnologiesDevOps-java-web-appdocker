node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/MithunTechnologiesDevOps/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t sushma994/java-web-app1 .'
    }
    
    stage('Push Docker Image'){
         withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
          sh "docker login -u sushma994 -p ${dockerhubpwd}"
        }
        sh 'docker push sushma994/java-web-app1'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app sushma994/java-web-app1'
         
         sshagent(['DOCKER_SERVER']) {
              sshagent(['dockerdevserver']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.44.206 docker stop java-web-app || true'
          sh 'ssh  ubuntu@172.31.44.206 docker rm java-web-app1 || true'
          sh 'ssh  ubuntu@172.31.44.206 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.44.206 ${dockerRun}"
       }
       
     }
   }   
 }
