pipeline {
    agent {label "flask"}
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/LondheShubham153/two-tier-flask-app.git", branch: "jenkins"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build . -t flask2"
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag flask2 ${env.dockerHubUser}/flask2:latest"
                    sh "docker push ${env.dockerHubUser}/flask2:latest" 
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
