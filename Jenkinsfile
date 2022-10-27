pipeline{
    agent any

    stages{
        stage ('Build Docker Image'){
            steps{
                script {
                    def dockerimg = docker.build("wesleymcarvalho/kube-news:${env.BUILD_ID}", "-f ./src/Dockerfile ./src")

                }
            }

        }
    }
}