pipeline {
    environment {
        imagename = "ganeshpoloju/jenkinss"
        registryCredential = 'dckr_pat_JAmd_CrVioeIykrKNE4hCTG90gk'
        dockerImage = ''
        containerName = 'my-container'
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git(url: 'https://github.com/GANESH0369/jenkins.git', branch: 'main')
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
                    sh "docker run -d --name ${containerName} ${imagename}:latest"
                    // Perform any additional steps needed while the container is running
                }
            }
        }
        stage('Stop and Remove Container') {
            steps {
                script {
                    sh "docker stop ${containerName} || true"
                    sh "docker rm ${containerName} || true"
                }
            }
        }
        stage('Deploy Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: registryCredential, usernameVariable: 'DOCKER_REGISTRY_USERNAME', passwordVariable: 'DOCKER_REGISTRY_PASSWORD')]) {
                        docker.withRegistry('https://index.docker.io/v1/', 'DOCKER_REGISTRY_USERNAME') {
                            dockerImage.push('latest')
                            dockerImage.push("${imagename}:${BUILD_NUMBER}")
                        }
                    }
                }
            }
        }
    }
}
