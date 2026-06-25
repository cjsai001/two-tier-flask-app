@Library("Shared") _
pipeline {
    
    agent { label "dev" };
    
    stages {
        stage("Code Clone") {
            steps {
                script {
                    clone("https://github.com/cjsai001/two-tier-flask-app.git", "main")
                }
            }
        }
        stage("Trivy File System Scanning") {
            steps {
                script {
                    trivy_fs()
                }
            }
        }
        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test") {
            steps {
                echo "Devloper / Tester will test"
            }
        }
        stage("Push to Docker Hub") {
            steps {
               script {
                   docker_push("dockerHubCreds","two-tier-flask-app")
               }
            }
        }
        stage("Deploy") {
            steps {
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

// Fixed function parameters for safety and credential masking
def docker_push(String credId, String imageName) {
    withCredentials([usernamePassword(
        credentialsId: "${credId}",
        passwordVariable: "dockerHubPass",
        usernameVariable: "dockerHubUser"
    )]) {
        sh "docker login -u \$dockerHubUser -p \$dockerHubPass"
        sh "docker image tag ${imageName} \$dockerHubUser/${imageName}"
        sh "docker push \$dockerHubUser/${imageName}:latest"
    }
}

// FIXED: Removed the space between the dash and the 'o' flag
def trivy_fs() {
    sh "trivy fs . -o results.json"
}
