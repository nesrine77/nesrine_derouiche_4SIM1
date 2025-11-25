pipeline {
    agent any
    environment {
        IMAGE_NAME = "nesrine77/student-management"  // change to your Docker Hub repo if different
        REGISTRY = "docker.io"
    }
    triggers {
        githubPush()
        pollSCM('H/5 * * * *')
    }
    stages {
        stage('Checkout') {
            steps {
                deleteDir()
                git branch: 'main', url: 'https://github.com/nesrine77/nesrine_derouiche_4SIM1.git'
            }
        }
        stage('Package') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw -B clean package'
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    def shortCommit = sh(script: 'git rev-parse --short=7 HEAD', returnStdout: true).trim()
                    env.IMAGE_TAG = "${shortCommit}-${env.BUILD_NUMBER}"
                }
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} -t ${IMAGE_NAME}:latest ."
            }
        }
        stage('Docker Push') {
            steps {
                // Requires Jenkins credential id 'dockerhub-creds' (username/password)
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo "Logging in to Docker Hub..."'
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker push ${IMAGE_NAME}:latest"
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo "Success: Image pushed -> ${REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}"
        }
        failure {
            echo 'Failure: check the build logs.'
        }
        unstable {
            echo 'Unstable: tests or quality gates failed.'
        }
    }
}