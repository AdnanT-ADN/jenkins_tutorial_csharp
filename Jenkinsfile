pipeline {

    agent any

    environment {
        DOCKER_IMAGE = "docker_csharp_test"
        DOCKER_TAG = "latest"
    }

    stages {

        stage("Checkout") {
            steps {
                checkout scm
            }
        }

    // stage("Set Up PATH") {
        //     steps {
        //         script {
        //             // env.PATH = "/home/adn/.asdf/installs/dotnet-core/8.0.403/./dotnet"
        //             // env.PATH = "/home/adn/.asdf/installs/dotnet-core/8.0.403/./dotnet:/usr/bin:/usr/local/bin:/bin"
        //             // env.PATH = "/home/adn/.asdf/installs/dotnet-core/8.0.403:/usr/local/bin:/usr/bin:/bin"
        //         }
        //     }
        // }

        stage("Build .NET Application") {
            steps {
                script {
                    // def dotnetPath = "/home/adn/.asdf/installs/dotnet-core/8.0.403/./dotnet"
                    // sh "dotnet clean"
                    dotnet "clean"
                    dotnet "restore"
                    dotnet "build --configuration Release"
                    dotnet "publish --configuration Release -o /App/out"
                    dotnet "publish --configuration Release -o ./publish"
                    // sh "dotnet restore"
                    // sh "dotnet build --configuration Release"
                    // sh "dotnet publish --configuration Release -o /App/out"
                    // sh "dotnet publish --configuration Release -o ./publish"
                }
            }
        }

        // TODO ADD Tests stage

        stage("Build Docker Image") {
            steps {
                script {
                    // docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}", "-f Dockerfile .")
                    docker "build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }

    }

    post {
        always {
            echo "Always Operation"
            script {
                docker "system prune -f || true"
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