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
                    dockerImage = docker.build imagename
                }
            }
        }

        stage('Running image') {
            steps {
                script {
                    sh "docker run ${imagename}:latest"
                }
            }
        }

        stage('Deploy Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push("$BUILD_NUMBER")
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}
