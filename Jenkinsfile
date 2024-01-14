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
                    def initialImageTag = "v${BUILD_NUMBER}"

                    for (folder in folderList) {
                        // Increment the tag for each iteration
                        def imageTag = initialImageTag
                        //initialImageTag = "v${BUILD_NUMBER + 1}" // Increment for the next folder
                        initialImageTag = "v" + (BUILD_NUMBER + 1)
                        // Create a stage for each folder
                        stage("Build ${folder} Docker Image And Push to DockerHub") {
                            // Navigate to the specified folder
                            dir(folder.trim()) {
                                // Build Docker image for each folder with an incremented tag
                                echo "Building Docker image for ${folder}"
                                sh "docker build -t ${DOCKER_REGISTRY}/${folder}:${imageTag} ."

                                withDockerRegistry(credentialsId: 'DockerHub', toolName: 'docker') {
                                    sh "docker push ${DOCKER_REGISTRY}/${folder}:${imageTag}"
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}
