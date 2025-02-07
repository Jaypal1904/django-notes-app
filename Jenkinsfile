@Library('Shared')_
pipeline {
    agent { label "demo-cicd" }

    stages {
        stage("Code") {
            steps {
                echo "THIS IS CLONING THE CODE"
                git url: "https://github.com/Jaypal1904/django-notes-app.git", branch: "main"
                echo "Code cloning successfully"
            }
        }
        
        stage("Build") {
            steps {
                echo "THIS IS BUILDING THE CODE"
                sh 'whoami'
                sh 'docker build -t notes-app:latest .'
            }
        }
        
        stage("Push to DockerHub") {
            steps {
                echo "THIS IS IMAGE PUSHING TO DockerHub"
                withCredentials([usernamePassword(
                    credentialsId: 'docker-jenkins',
                    passwordVariable: 'dockerHubPass', 
                    usernameVariable: 'dockerHubUser'
                )]) {
                    sh 'echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin'
                    sh 'docker tag notes-app:latest jaypal1904/notes-app:latest'
                    sh 'docker push jaypal1904/notes-app:latest'
                }
            }
        }
        
        stage("Deploy") {
            steps {
                echo "THIS IS DEPLOYING THE CODE"
                sh 'docker run -d -p 8000:8000 notes-app:latest'
            }
        }
    }
}

}
