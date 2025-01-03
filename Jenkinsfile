
pipeline {
    agent any

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
                    sh 'docker build -t todo_app .'
                }
            }
        }
        stage("Push to dockerhub") {
            steps {
                echo "Pushing to dockerhub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubpass", usernameVariable: "dockerHubUser")]) {
                    sh "docker tag todo_app ${env.dockerHubUser}/todo_app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubpass}"
                    sh "docker push ${env.dockerHubUser}/todo_app:latest"
                }
            }
        }
        stage('Deploy') {
            steps {  
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
