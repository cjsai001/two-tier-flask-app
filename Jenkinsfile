@Library("Shared") _
pipeline{
    
    agent { label "dev"};
    
    stages{
        stage("Code Clone"){
            steps{
                scripts{
                    clone("
            }
        }
        stage("Trivy File System Scanning"){
            steps{
                sh 'trivy fs . -o results.json'
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test"){
            steps{
                echo "Devloper / Tester will test"
            }
            
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                    )]){
                    
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"

                    }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
post {
    success {
        emailext(
        subject: "Build Successful", 
        body: "Good News: your build was successful!",
        to: 'chilamakurijayasimhasai@gmail.com'
        )
    }
    failure {
        emailext(
        subject: "Build Failed",
        body: "Bad News: Your build Failed!",
        to: 'chilamakurijayasimhasai@gmail.com'
        )
     }
   }
}
