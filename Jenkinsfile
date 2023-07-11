pipeline {
    agent any

    stages{
        stage ("Clone code") {
            steps {
                echo "cloning the code"
                git url:"https://github.com/FuzailN/my-java-app.git", branch: "master"
            }
        }

        stage('maven-buildstage'){
            steps {
                script {
                    def mvnHome =  tool name: 'maven3', type: 'maven'
                    sh "${mvnHome}/bin/mvn clean package"
                    sh 'mv target/myweb*.war target/newapp.war'
        }
    }
}
stage ("Build") {
            steps {
                echo "Building the image"
                sh "docker build -t myweb ."
                }
            }
            stage("Push to Docker Hub"){
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag myweb ${env.dockerHubUser}/myweb:0.0.2"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/myweb:0.0.2"
                }
            }
        }
         stage ("Deploy") {
            steps {
                echo "Deploying the container"
                sh 'docker run -d -p 8090:8080 --name tomcattest imfuzail/myweb:0.0.2'
                
            }
        }
}
}
