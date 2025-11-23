pipeline {
    agent any
    stages {
        stage('Clone Repo') {
            steps {
                git url: 'https://github.com/iddhawan01/Application-on-Docker-Container.git', branch: 'main'
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t public-container .'
            }
        }
        stage('Run Container') {
            steps {
                sh 'docker rm -f public-container || true'
                sh 'docker run -d -p 8081:80 --name public-container public-container'
            }
        }
    }
}