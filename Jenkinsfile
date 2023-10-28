pipeline{
    agent any
    stages{
        stage("Cloning the Code"){
            steps{
                echo "Cloning the code..."
                git url:"https://github.com/MudabbirulSaad/django-notes-app.git", branch:"main"
            }
        }
        stage("Build"){
            steps{
                echo "Building the image..."
                sh "docker build -t notes-app ."
            }
        }
        stage("Push to Docker"){
            steps{
                echo "Pushing the image to docker..."
                withCredentials([usernamePassword(credentialsId:"dockercreds", passwordVariable:"dockercredsPass", usernameVariable:"dockercredsUser")]){
                    sh "docker tag notes-app ${env.dockercredsUser}/notes-app:latest"
                    sh "docker login -u ${env.dockercredsUser} -p ${env.dockercredsPass}"
                    sh "docker push ${env.dockercredsUser}/notes-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the container..."
                withCredentials([usernamePassword(credentialsId:"dockercreds", passwordVariable:"dockercredsPass", usernameVariable:"dockercredsUser")]){
                    sh "docker-compose down && docker-compose up -d"
                }
            }
        }
    }
}
