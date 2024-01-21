pipeline {
    agent any
      options {
        docker.enabled true
    }
    environment {
        DOCKER_IMAGE = 'test3'
        CONTAINER_NAME = 'jhgfd'
        PORT_MAPPING = '8089:8089'  // Adjust the port mapping as needed
    }

    stages {
        // stage('Checkout') {
        //     steps {
        //         // Clean workspace before checkout
        //         deleteDir()
        //         // Checkout the HTML source code from GitHub
        //         git url: 'https://github.com/andrinahaura/project1.git'
        //     }
        // }
            stage('Checkout') {
                steps {
                    deleteDir()
                    checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://github.com/ilham275/testdeploy.git']]])
                          // Tambahkan pernyataan log untuk menampilkan direktori saat ini
                }
            }

        stage('Build Docker Image') {
            steps {
                script {
                      sh 'ls -l'
                    dir('testdeploy') {
                        // Build Docker image dengan konten HTML
                        echo 'Memulai pembangunan Docker image'
                        bat 'docker build -t test3 -f Dockerfile .'
                        // docker.build("${DOCKER_IMAGE}", '-f Dockerfile .')
                        echo 'Selesai pembangunan Docker image'
                    }
                    // // Build Docker image with the HTML content
                    // docker.build("${DOCKER_IMAGE}", '-f Dockerfile .')
                }
            }
        }

    
        stage('Run Docker Container') {
            steps {
                script {
                    // Run Docker container based on the built image
                    docker.image("${DOCKER_IMAGE}").run("-p ${PORT_MAPPING} --name ${CONTAINER_NAME}")
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