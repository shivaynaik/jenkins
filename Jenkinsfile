pipeline {
    environment {
        imagename = "ganeshpoloju/jenkinss"
        registryCredential = 'Ganesh@1998'
        dockerImage = ''
    }
 
    agent any
 
    stages {
        stage('Cloning Git') {
            steps {
                git([url: 'https://github.com/GANESH0369/jenkins.git', branch: 'main'])
            }
        }
 
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build "${imagename}:latest"
                }
            }
        }
 
        stage('Running image') {
            steps {
                script {
                    docker.image("${imagename}:latest").run("--rm -p 8080:8080", detach: true)
                }
            }
        }
 
        stage('Deploy Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }
}
