pipeline {
    agent any
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
                    // Build Docker image
                    sh "docker build -t ${DOCKER_IMAGE} -f Dockerfile ."
            }
        }

          stage('Build Docker Image') {
            steps {
                    // Build Docker image
                    sh "docker run --name web_server -d -p 80100:80 web"
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
