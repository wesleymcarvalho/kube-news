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

        stage ('Push Docker Image'){
            steps{
                script{
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
                        dockerimg.push('latest')
                        dockerimg.push("${env.BUILD_ID}")
                    }                    
                }
            }

        }
    }
}