pipeline{
    agent any

    stages{
        environment{
            tag_version = "v${env.BUILD_ID}"
        }
        stage ('Build Docker Image'){
            steps{
                script {
                    dockerimg = docker.build("wesleymcarvalho/kube-news:${tag_version}", "-f ./src/Dockerfile ./src")
                }
            }
        }

        stage ('Push Docker Image'){
            steps{
                script{
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
                        dockerimg.push('latest')
                        dockerimg.push("${tag_version}")
                    }                    
                }
            }
        }

        stage ('Deploy Kubernets') {
            steps{
                withKubeConfig([credentialsId: 'kube_config']){
                    sh 'sed -i "s/{{TAG_VERSAO}}/$tag_version/g" ./k8s/Deployment.yaml'
                    sh 'sed -i "s/{{TAG_VERSAO}}/$tag_version/g" ./src/views/partial/nav-bar.ejs'
                    sh 'kubectl apply -f ./k8s/Deployment.yaml'
                }

            }
        }
    }
}