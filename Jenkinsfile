pipeline {

    agent any

    options {
        disableConcurrentBuilds()
    }

    environment {
        IMAGE_TAG           = "v0.0.0"
        IMAGE_NAME          = "azamaschikov/greetings:${env.IMAGE_TAG}"

        REPOSITORY_BRANCH   = "main"
        REPOSITORY_ADDRESS  = "https://github.com/azamaschikov/Greetings"

        REGISTRY_CREDENTIAL = "9e21e98d-07fd-4511-8fd6-5421de60a916"
    }

    stages {

        stage("Clean Working Directory") {
            steps {
                cleanWs()
            }
        }

        stage("Clone Repository") {
            steps {
                checkout([$class: "GitSCM", branches: [[name: REPOSITORY_BRANCH]],
                userRemoteConfigs: [[url: REPOSITORY_ADDRESS]]])
            }
        }

        stage("Build Docker Image") {
            steps {
                sh "docker build --tag ${env.IMAGE_NAME} ."
            }
        }

        stage("Docker Push Image") {
            steps {
                withCredentials([usernamePassword(credentialsId: REGISTRY_CREDENTIAL, usernameVariable: "USERNAME", passwordVariable: "PASSWORD")]) {
                    sh "docker login --username ${env.USERNAME} --password ${env.PASSWORD}"
                    sh "docker push ${env.IMAGE_NAME}"
                }
            }
        }

    }
}
