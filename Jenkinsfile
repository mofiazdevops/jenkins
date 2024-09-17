pipeline{
    agent any
    
    stages{
        stage("Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/mofiazdevops/jenkins.git", branch: "main" 
            }
        }
        stage("build"){
            steps {
                echo "Building the image"
                sh "docker-compose down && docker-compose up -d"
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPassword",usernameVariable:"dockerHubUser")]){
                sh "docker tag fiaz-image ${env.dockerHubUser}/fiaz-image:v1"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                sh "docker push ${env.dockerHubUser}/fiaz-image:v1"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the containe"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
