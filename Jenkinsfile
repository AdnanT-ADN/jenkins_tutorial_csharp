pipeline {

    agent any

    environment {
        DOCKER_REGISTRY = "MY_REGISTRY"
        DOCKER_IMAGE = "MY_REGISTRY/DOCKER_C_SHARP"
        DOCKER_TAG = "latest"
    }

    stages {

        stage("Checkout") {
            steps {
                checkout scm
            }
        }

        stage("Build .NET Application") {
            steps {
                script {
                    sh "dotnet clean"
                    sh "dotnet restore"
                    sh "dotnet build --configuration Release"
                    sh "dotnet publish --configuration Release -o ./publish"
                }
            }
        }

        // TODO ADD Tests stage

        stage("Build Docker Image") {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}", "-f Dockerfile .")
                }
            }
        }

    }

    always {
        sh "docker system prune -f"
    }

    success {
        echo "Feature Build Successful."
    }
    
    failure {
        echo "Feature Build Unsuccessful."
    }

}