pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        checkout scm
        sh "chmod +x -R ${env.WORKSPACE}"
      }
    }

    stage('Application Build') {
      steps {
        sh 'scripts/build.sh'
      }
    }

    stage('Application Test') {
      steps {
        sh 'scripts/test.sh'
      }
    }

    stage('Docker image build') {
      steps {
        sh 'docker build -t a13xg0/gorbach_cicd:$BUILD_NUMBER .'
      }
    }

    stage('Docker image deploy') {
      steps {
        script {
          docker.withRegistry('', 'dockerhub-cicd')
          {
            docker.image("a13xg0/gorbach_cicd:$env.BUILD_NUMBER").push("latest")
          }
        }

      }
    }

  }
}
