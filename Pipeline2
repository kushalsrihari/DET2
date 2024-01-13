pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/kushalsrihari/DET2.git'
        PROJECT_FOLDERS = 'cart,catalogue,dispatch,mongo,mysql,payment,ratings,shipping,user,web'
        DOCKER_REGISTRY = 'kushalsrihari' // Replace with your DockerHub username or organization
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Clone the GitHub repository
                    git branch: 'main', url: REPO_URL
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    def folderList = PROJECT_FOLDERS.split(',')

                    for (folder in folderList) {
                        // Create a stage for each folder
                        stage("Build ${folder} Docker Image") {
                            // Navigate to the specified folder
                            dir(folder.trim()) {
                                // Build Docker image for each folder
                                echo "Building Docker image for ${folder}"
                                // Corrected Docker build command
                                sh "docker build -t ${DOCKER_REGISTRY}/${folder}:latest ."
                            }
                        }
                    }
                }
            }
        }

        stage('Push Docker Images to DockerHub') {
            steps {
                script {
                    def folderList = PROJECT_FOLDERS.split(',')

                    for (folder in folderList) {
                        // Push Docker image for each folder to DockerHub
                        withDockerRegistry(credentialsId: 'DockerHub', toolName: 'docker') {
                            sh "docker push ${DOCKER_REGISTRY}/${folder}:latest"
                        }
                    }
                }
            }
        }
    }
}
