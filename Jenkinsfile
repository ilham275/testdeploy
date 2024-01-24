pipeline {
    agent {
        docker {
            // Gunakan gambar Docker yang mendukung build Anda
            image 'docker:24.0.6'  // Ganti dengan versi Docker yang sesuai
            args '-v /var/run/docker.sock:/var/run/docker.sock'  // Mount Docker socket agar bisa menggunakan Docker dari dalam Docker
        }
    }
    
    environment {
        DOCKER_IMAGE = 'test3'
        CONTAINER_NAME = 'jhgfd'
        PORT_MAPPING = '8081:80'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Clean workspace before checkout
                    deleteDir()
                    checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://github.com/ilham275/testdeploy.git']]])

                    // Capture the current directory
                    def currentDir = sh(script: 'pwd', returnStdout: true).trim()
                    echo "Current Directory: ${currentDir}"

                    // Set the environment variable for later use
                    env.CURRENT_DIR = currentDir
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir(env.CURRENT_DIR) {
                    // Build Docker image
                    sh "docker build -t ${DOCKER_IMAGE} -f Dockerfile ."
                }
            }
        }
    }

    post {
        always {
            script {
                // Stop and remove the Docker container after execution
                docker.image("${DOCKER_IMAGE}").stop()
                docker.image("${DOCKER_IMAGE}").remove()
            }
        }
    }
}
