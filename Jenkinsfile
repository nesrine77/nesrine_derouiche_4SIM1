pipeline {
    agent any
    triggers {
        githubPush()
        pollSCM('H/5 * * * *')  // Poll SCM every 5 minutes
    }
    stages {
        stage('Checkout') {
            steps {
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
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Build successful! Application packaged successfully.'
        }
        failure {
            echo 'Build failed! Check the logs for errors.'
        }
        unstable {
            echo 'Build unstable. Tests may have failed.'
        }
    }
}