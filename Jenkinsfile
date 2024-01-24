pipeline {
    agent any

    stages {
        stage('Print PATH') {
            steps {
                script {
                    sh 'echo $PATH'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t nama_image:tag -f path/to/Dockerfile .'
                }
            }
        }
    }
}
