
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'my_notes_app'
    }

    stages {
        stage("Clone code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/Madhu9209/django-notes-app.git", branch: "main"
            }
        }
    }
    stage('Build') {
            steps {
                echo "Building the Docker image"
                script {
                    sh 'docker --version'
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }
}
