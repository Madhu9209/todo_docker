
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
        echo "Deploy the container"
        script {
            // Check if the container is running, then stop and remove it
            def containerExists = sh(script: "docker ps -q -f name=todo_app", returnStdout: true).trim()
            if (containerExists) {
                sh "docker stop todo_app"
                sh "docker rm todo_app"
            } else {
                echo "No running container named 'todo_app' found"
            }

            // Run the new container
            sh "docker run -d -p 1318:1318 madhu220/todo_app:latest"
        }
    }
}

    }
}
