pipeline {
    
    agent any
    tools {
        git 'git'
    }
    
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/Muktamk/django-notes-app.git", branch: "main"
                echo "success"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build . -t mukta178/node_js:latest"
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials(
                    [usernamePassword(
                        credentialsId:"dockerCreds",
                        passwordVariable:"dockerHubPass", 
                        usernameVariable:"dockerHubUser"
                        )
                    ]
                ){
                sh "docker image tag notes-app-jenkins:latest ${env.dockerHubUser}/notes-app-jenkins:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app-jenkins:latest"
                }
            }
        }
        
        stage("Deploy"){
            steps{
                sh "docker compose up -d"
            }
        }
    }
}
