pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Nilaswathi/cicd-demo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo Build completed'
            }
        }

        stage('SonarQube Scan') {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner'
                    withSonarQubeEnv('sonar-server') {
                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=cicd-demo \
                        -Dsonar.projectName=cicd-demo \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://44.204.29.141:9000
                        """
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t cicd-demo .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker stop cicd-demo-container || true'
                sh 'docker rm cicd-demo-container || true'
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d -p 80:80 --name cicd-demo-container cicd-demo'
            }
        }

    }
}
