pipeline{
    
    agent { label "dev"};
    
    stages{
        stage("Code Clone"){
            steps{
                git url: "https://github.com/cjsai001/two-tier-flask-app", branch: "main"
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
        emailtext(
        Subject: "Build Successful",
        body: "good News: your build was successful!",
        to: 'chilamakurijayasimhasai@gmailcom
        )
    }
    failure {
        emailtext(
        subject: "Build Failed",
        body: "Bad News: Your build Failed!",
        to: 'chilamakurijayasimhasai@gmail.com'
        )
     }
   }
}
