pipeline{
    agent { label 'dev' }
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/Paribhasha1111/two-tier-flask-app.git" , branch : "master"
                echo "code clone ho gya git se"
                
            }
        }
        stage("cleanup"){
            steps{
                sh "docker system prune -f"
            }
        }
        stage("Build"){
            steps{
                
                echo "code build ho gya docker se"
                sh "docker build -t two-tier-flask-app ."
            
            }
        }
        stage("Test"){
            steps{
                echo "developer/tester test likh ke dega"
                
            }
        }
        stage("push to dockerhub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCred" , 
                    passwordVariable:"dockerHubPass" , 
                    usernameVariable :"dockerHubUser"
                    )]){
                  sh "docker login -u${env.dockerHubUser} -p${env.dockerHubPass}"
                  sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                  sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
                
            }
        }
        stage("Clean Running Containers") {
           steps {
              sh "docker rm -f flask-app || true"
          }
       }

        stage("Deploy"){
            steps{
                echo "deploy ho gya docker compose se"
                sh "docker compose up -d --build"
            }
        }
    } 
}    
