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
                    dockerImage = docker.build "${imagename}:${BUILD_NUMBER}"
                }
            }
        }

        stage('Running image') {
            steps {
                script {
                    sh "docker run ${imagename}:${BUILD_NUMBER}"
                }
            }
        }

        stage('Deploy Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
                        dockerImage.push("${BUILD_NUMBER}")
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}
