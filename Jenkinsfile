node {
    stage('Checkout') {
        deleteDir()
        git branch: 'main', 
            url: 'https://github.com/nesrine77/nesrine_derouiche_4SIM1.git'
    }
    stage('Build') {
        sh 'chmod +x mvnw'
        sh './mvnw -B clean compile'
    }
    stage('Test') {
        sh 'chmod +x mvnw'
        sh './mvnw -B test'
    }
    stage('Package') {
        sh 'chmod +x mvnw'
        sh './mvnw -B package -DskipTests'
    }
}