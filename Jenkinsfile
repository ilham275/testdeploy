pipeline {
    agent any

    environment {
        PATH = "/opt/java/openjdk/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/bin/docker"
        DOCKER_IMAGE = 'test3'
        CONTAINER_NAME = 'jhgfd'
        PORT_MAPPING = '8081:80'  // Sesuaikan port mapping sesuai kebutuhan
    }

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
                    sh 'docker build -t ${DOCKER_IMAGE} -f Dockerfile .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}").run("-p ${PORT_MAPPING} --name ${CONTAINER_NAME}")
                }
            }
        }
    }

    post {
        always {
            script {
                docker.image("${DOCKER_IMAGE}").stop()
                docker.image("${DOCKER_IMAGE}").remove()
            }
        }
    }
}
