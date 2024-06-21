pipeline {
    environment {
        imagename = "shivaynaik/jenkinss"
        dockerImage = ''
        containerName = 'my-container'
        dockerHubCredentials = 'shivaynaik'
        dockerImageTag = "${imagename}:${env.BUILD_NUMBER}"
    }

    agent any

    stages {
        stage('Cloning Git') {
            steps {
                git(url: 'https://github.com/shivaynaik/jenkins.git', branch: 'main')
            }
        }

        stage('Building image') {
            steps {
                script {
                    dockerImageTag = "${imagename}:${env.BUILD_NUMBER}"
                    dockerImage = docker.build dockerImageTag
                }
            }
        }

        stage('Running image') {
            steps {
                script {
                    sh "docker run -d --name ${containerName} ${dockerImageTag}"
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
                    withCredentials([usernamePassword(credentialsId: 'DOCKER_REGISTRY_CREDS' , usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                        sh "docker push ${dockerImageTag}"
                    }
                }
            }
        }

        stage('Trigger ManifestUpdate') {
            steps {
                echo "Triggering updatemanifestjob"
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
                }
            }
        }
    }
}
