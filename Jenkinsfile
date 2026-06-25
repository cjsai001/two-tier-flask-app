@Library("Shared") _
pipeline{
    
    agent { label "dev"};
    
    stages{
        stage("Code Clone"){
            steps{
                script{
                    clone("https://github.com/cjsai001/two-tier-flask-app.git", "main")
                }
            }
        }
        stage("Trivy File System Scanning"){
            steps{
                script{
                    trivy_fs()
                }
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
               script{
                   docker_push("dockerHubCreds","two-tier-flask-app")
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
