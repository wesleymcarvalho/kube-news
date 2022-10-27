pipeline{
    agent any

    stages{
        stage ('Build Docker Image'){
            steps{
                script {
                    dockerimg = docker.build("wesleymcarvalho/kube-news:${env.BUILD_ID}", "-f ./src/Dockerfile ./src")
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

        stage ('Deploy Kubernets') {
            environment{
                tag_version = "${env.BUILD_ID}"
            }
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