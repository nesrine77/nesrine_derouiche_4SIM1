pipeline {
    agent any
    triggers {
        githubPush()
    }
    stages {
        stage('Checkout') {
            steps {
                deleteDir()
                git branch: 'main', 
                    url: 'https://github.com/nesrine77/nesrine_derouiche_4SIM1.git'
            }
        }
        stage('Build') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw -B clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw -B test'
            }
        }
        stage('Package') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw -B package -DskipTests'
            }
        }
    }
    post {
        success {
            echo 'Build successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}