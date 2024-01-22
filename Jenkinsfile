pipeline {
    environment {
        imagename = "ganeshpoloju/jenkins"
        registryCredential = 'dckr_pat_JAmd_CrVioeIykrKNE4hCTG90gk'
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
       stage('Deploy Image') {
            steps {
                script {
                    // Login to Docker Hub
                    sh "docker login -u ganesh -p Ganesh@1998"
 
                    // Push the image
                    sh "docker push ${imagename}:latest"
                }
            }
        }
    
        stage('Remove Unused docker image') {
            steps {
                sh "docker rmi $imagename:$BUILD_NUMBER"
                sh "docker rmi $imagename:latest"
            }
        }
    }
}





