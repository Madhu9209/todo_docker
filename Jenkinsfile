
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'my_notes_app'
    }

    stages {
        stage("Clone code") {
            steps {
                echo "Cloning the code"
                git url:"https://github.com/Madhu9209/todo_docker.git", branch: "main"
            }
        }
    stage("Build") {
            steps {
                echo "Building the Docker image"
                script {
                    sh 'docker --version'
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }
    }
    stage("Push to dockerhub") {
            steps {
                echo "Pushing to dockerhub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubpass", usernameVariable: "dockerHubUser")]) {
                    echo "Using DockerHub user: ${env.dockerHubUser}"  // For debugging
                    script {
                        sh """
                            docker tag ${DOCKER_IMAGE} ${env.dockerHubUser}/${DOCKER_IMAGE}:latest
                            echo ${env.dockerHubpass} | docker login -u ${env.dockerHubUser} --password-stdin
                            docker push ${env.dockerHubUser}/${DOCKER_IMAGE}:latest
                        """
                    }
                }
            }
        }
}
