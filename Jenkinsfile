pipeline {

    agent any

    environment {
        DOCKER_REGISTRY = "MY_REGISTRY"
        DOCKER_IMAGE = "docker_csharp_test"
        DOCKER_TAG = "latest"
    }

    stages {

        stage("Checkout") {
            steps {
                checkout scm
            }
        }

        stage("Set Up PATH") {
            steps {
                script {
                    env.PATH = "/home/adn/.asdf/installs/dotnet-core/8.0.403/./dotnet"
                }
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
                    // docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}", "-f Dockerfile .")
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }

    }

    post {
        always {
            echo "Always Operation"
            script {
                sh "docker system prune -f"
            }
        }

        success {
            echo "Feature Build Successful."
        }
        
        failure {
            echo "Feature Build Unsuccessful."
        }
    }


}