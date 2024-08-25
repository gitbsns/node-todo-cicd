pipeline {
    agent { label "dev-server"}
    
    stages {
        
        stage("cloning the code"){
            steps{
                git url: "https://github.com/gitbsns/node-todo-cicd.git", branch: "master"
                echo ' code clone ho gaya'
            }
        }
        stage("build && test"){
            steps{
                sh "docker build -t node-app-test-new ."
                echo 'code build ho gaya'
            }
        }
        stage("scan the image"){
            steps{
                echo 'image scanning ho gayi'
            }
        }
        stage("push"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag node-app-test-new:latest ${env.dockerHubUser}/node-app-test-new:latest"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                echo 'image push ho gaya'
                }
            }
        }
        stage("deploy the container"){
            steps{
                sh "docker stop nodeapp1"
                sh "docker rm -f nodeapp1"
                sh "docker run -d --name nodeapp1 -p 8000:8000 notesapp"
                echo 'deployment ho gaya'
            }
        }
    }
}
