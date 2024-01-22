pipeline {
    agent any
    
    stages {
        stage('Clone and Build') {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://github.com/Narsi12/fastapi_helloworld_cicd.git']]])
                    docker.build("ganeshpoloju/jenkinss:latest")
                }
            }
        }
        stage('Deploy Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dckr_pat_JAmd_CrVioeIykrKNE4hCTG90gk') {
                        docker.image("ganeshpoloju/jenkinss:latest").push()
                        docker.image("ganeshpoloju/jenkinss:${BUILD_NUMBER}").push() // Tag with Jenkins build number
                    }
                }
            }
        }
    }
}
