node{
   def buildNumber = BUILD_NUMBER 
    stage("git clone"){
        git url: "https://github.com/visureddy247/java-web-app-docker.git", branch: "master"
    }
    stage("maven clean package"){
     def mavenHome= tool name: "maven", type: "maven"
     sh "${mavenHome}/bin/mvn clean package"
}
    stage("build docker image"){
        sh "docker build -t visureddy247/java_web_app_docker:${buildNumber} ."
    }
    stage("docker login nd push"){
      withCredentials([string(credentialsId: 'dockerhubpwD', variable: 'dockerhubpwD')]) {
   sh "docker login -u visureddy247 -p ${dockerhubpwD}"
    }
  sh "docker push visureddy247/java_web_app_docker:${buildNumber}"
           }
           
    stage("deploy application inas docker server"){
           sshagent(['dockerdevserver']) {
              sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.11.191 docker rm -f javawebappcontainer || true"
              
              sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.11.191 docker run -d -p 8080:8080 --name javawebappcontainer visureddy247/java_web_app_docker:${buildNumber} "
}
    }        
}
