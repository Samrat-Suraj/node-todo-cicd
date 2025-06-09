pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "samratsooraj/node_app_practice:latest"
    }

    stages {
        stage("Code Clone") {
            steps {
                echo "Code Clone Stage"
                git url: "https://github.com/Samrat-Suraj/node-todo-cicd.git", branch: "master"
            }
        }

        stage("Code Build & Test") {
            steps {
                echo "Code Build Stage"
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage("Push To DockerHub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHub",
                    usernameVariable: "DOCKER_USER", 
                    passwordVariable: "DOCKER_PASS"
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker compose down || true"
                sh "docker compose up -d --build"
            }
        }
    }
}
