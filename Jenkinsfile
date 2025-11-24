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
                sh './mvnw -B clean compile'
            }
        }
        stage('Test') {
            steps {
                sh './mvnw -B test'
            }
        }
        stage('Package') {
            steps {
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