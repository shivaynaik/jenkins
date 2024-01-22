pipeline {
    environment {
        imagename = "ganeshpoloju/jenkinss"
        registryCredential = 'dckr_pat_JAmd_CrVioeIykrKNE4hCTG90gk'
        dockerImage = ''
        containerName = '48b5285f1592'
        AWS_ACCESS_KEY_ID     = credentials('AKIARY57NE7BG2462KVZ')
        AWS_SECRET_ACCESS_KEY = credentials('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')
        S3_BUCKET             = 'cicd-mt'
        DOCKER_IMAGE_NAME     = 'ganeshpoloju/jenkinss '
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                script {
                    git branch: 'main', url: 'https://github.com/Narsi12/fastapi_helloworld_cicd.git'
                }
            }
        }
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build "${imagename}:latest"
                }
            }
        }
        stage('Push Docker Image to S3') {
            steps {
                script {
                    sh "docker save ${imagename}:latest | gzip | aws s3 cp - s3://${S3_BUCKET}/${DOCKER_IMAGE_NAME}.tar.gz"
                }
            }
        }
    }
}
