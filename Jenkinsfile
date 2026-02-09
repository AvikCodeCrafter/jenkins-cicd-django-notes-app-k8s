pipeline {
    agent any

    stages {

        stage('Code Clone') {
            steps {
                echo 'Cloning the Code'
                git branch: 'main',
                    credentialsId: 'jenkins-github',
                    url: 'https://github.com/AvikCodeCrafter/django-notes-app.git'
            }
        }

        stage('Build Image') {
            steps {
                echo 'Building the Image'
                sh 'docker build -t django-notes-app .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing the Image to Docker Hub'
                withCredentials([usernamePassword(credentialsId: "dockerhub",passwordVariable: "dockerhubpass",usernameVariable: "dockerhubuser")]){
                sh "docker tag django-notes-app ${env.dockerhubuser}/django-notes-app:latest"    
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker push ${env.dockerhubuser}/django-notes-app:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the Application as Container into production'
                //sh "docker run -d -p 8000:8000 thatgeekcontainer/django-notes-app:latest"
                sh "docker-compose up -d"
            }
        }
    }
}
