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
         stage('Push Docker Image') {
            steps {
                script {
                    // Ensure docker is configured correctly in Jenkins
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        echo 'Pushing Docker Image to registry'
                        sh 'docker push mukta178/node_js:latest'
                    }
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
