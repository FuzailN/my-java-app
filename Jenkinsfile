node{
   stage('SCM Checkout'){
     git 'https://github.com/FuzailN/my-java-app.git' 
   }
      stage('maven-buildstage'){
	script {
      	def mvnHome =  tool name: 'maven3', type: 'maven'   
      	sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
      }
   stage('Build Docker Image'){
   sh 'docker build -t imfuzail/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u imfuzail -p ${dockerPassword}"
    }
   sh 'docker push imfuzail/myweb:0.0.2'
   }
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest imfuzail/myweb:0.0.2' 
   }
}