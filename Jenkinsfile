pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/Nilaswathi/cicd-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t cicd-demo .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker rm -f cicd-container || true'
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d -p 80:80 --name cicd-container cicd-demo'
            }
        }

    }
}
