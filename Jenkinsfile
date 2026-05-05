pipeline{
    agent any;

    stages{

     stage("Stop Old Container") {
    steps {
        echo "hello" 
                 sh 'docker stop flaskapp || true'
                 sh 'docker rm flaskapp || true'
                }
            }

        stage("Code Clone"){
            steps{
                git url: "https://github.com/anshumanpate/flask-app-ecs.git", branch: "main"
            }
        }

        stage("Build Docker Image"){
            steps{
                sh "docker build -t my-app ."
            }
        }

        stage("Tag & Push Docker Image"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubcred",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                )]) {

                    sh "docker login -u ${dockerHubUser} -p ${dockerHubPass}"

                    sh "docker tag my-app ${dockerHubUser}/my-app:latest"

                    sh "docker push ${dockerHubUser}/my-app:latest"
                    sh "docker run -d -p 80:80 --name flaskapp ${dockerHubUser}/my-app:latest"
                }
            }
        }

        
    }
}
