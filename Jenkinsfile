
pipeline {
    agent any
 
    environment {
        imagename = "ganeshpoloju/jenkinss"
        registryCredential = 'Ganesh@1998'
        dockerImage = ''
        versionTag = 'v1.0.0'  // Specify the version or tag you want to use
    }
 
    stages {
        stage('Clone Repository') {
            steps {
                script {
                    git(url: 'https://github.com/GANESH0369/jenkins.git', branch: 'main')
                }
            }
        }
 
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${imagename}:${versionTag}")
                }
            }
        }
 
        stage('Run Docker Image') {
            steps {
                script {
                    dockerImage.inside("-p 8081:8080") {
                        sh 'uvicorn app:app --host 0.0.0.0 --port 8080'
                    }
                }
            }
        }
 
        stage('Push to DockerHub') {
            steps {
                script {
                    withDockerRegistry([credentialsId: "${registryCredential}", url: "https://index.docker.io/v1/"]) {
                        dockerImage.push("${imagename}:${versionTag}")
                        docker.withRegistry("https://index.docker.io/v1/", "${registryCredential}") {
                            dockerImage.push("${imagename}:${versionTag}")
                        }
                    }
                }
            }
        }
    }
}
