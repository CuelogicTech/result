pipeline { 
  agent any
  tools {
    nodejs 'nodejs-6.x'
  }
 
  stages {
    stage ('Checkout Code') {
      steps {
        checkout scm
      }
    }
    stage ('Build app') {
      steps {
        sh "npm install"
      }
    }
    stage('Nexus login') {
        steps {
            sh 'sudo docker login localhost:5000 -u admin -p admin123'
        }
    }
    stage('Docker build') {
        steps {
            sh 'env'
            sh "sudo docker build -t localhost:5000/${env.JOB_NAME}:${env.BUILD_NUMBER}.0 ."
        }
    }
    stage('Docker push') {
        steps {
            sh 'env'
            sh "sudo docker push localhost:5000/${env.JOB_NAME}:${env.BUILD_NUMBER}.0"
        }
    }
    stage ('Clean') {
      steps {
        sh "npm prune"
        sh "rm -rf node_modules"
      }
    }
  }
}
