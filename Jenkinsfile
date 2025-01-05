pipeline {
  agent any

  environment {
    DOCKER_CREDENTIALS = credentials('docker-hub')
    DOCKER_IMAGE_NAME = 'abesk/cicd_practice'
  }

  stages {

    stage('Run Build Script') {
      steps {
        script {
          sh 'chmod +x ./scripts/build.sh'
          sh './scripts/build.sh'
        }
      }
    }

    stage('Run Test Script') {
      steps {
        script {
          sh 'chmod +x ./scripts/test.sh'
          sh './scripts/test.sh'
        }
      }
    }

    stage('Login to Docker Hub') {
      steps {
        script {
          sh 'docker login -u $DOCKER_CREDENTIALS_USR -p $DOCKER_CREDENTIALS_PSW'
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          sh 'docker --version'
          sh "docker build -t $DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."
        }
      }
    }

    stage('Push Docker Image') {
      steps {
        script {
          sh "docker push $DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
        }
      }
    }
  }

  post {
    always {
      sh 'docker system prune -f'
    }

    success {
      echo 'Docker image successfully pushed to Docker Hub!'
    }

    failure {
      script {
        echo 'Build failed. Capturing error output:'
        sh 'cat build.log test.log'
      }
    }
  }
}
