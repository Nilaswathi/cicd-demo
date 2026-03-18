pipeline {

 agent any

 environment {
  DOCKER_IMAGE = "nilavarasan/cicd-demo"
 }

 stages {

  stage('Checkout') {
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
    withSonarQubeEnv('sonar-scanner') {
     script {
      def scannerHome = tool 'sonar-scanner'
       sh """
        ${scannerHome}/bin/sonar-scanner \
        -Dsonar.projectKey=cicd-demo \
        -Dsonar.sources=.
         """
     }
    }
   }    
  }

  stage('Docker Build') {
   steps {
    sh 'docker build -t $DOCKER_IMAGE .'
   }
  }

  stage('Docker Login') {
   steps {
    withCredentials([usernamePassword(
      credentialsId: 'dockertoken',
      usernameVariable: 'USER',
      passwordVariable: 'PASS'
    )]) {
      sh 'echo $PASS | docker login -u $USER --password-stdin'
    }
   }
  }

  stage('Push Image') {
   steps {
    sh 'docker push $DOCKER_IMAGE'
   }
  }

  stage('Deploy Container') {
   steps {
    sh 'docker rm -f cicd-container || true'
    sh 'docker run -d -p 80:80 --name cicd-container $DOCKER_IMAGE'
   }
  }

 }
}
