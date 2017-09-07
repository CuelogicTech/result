pipeline { 
  agent any

  environment {
    NEXUS_URL = credentials('nexus_url')
    NEXUS = credentials('nexus')
  }

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
            sh "sudo docker login $NEXUS_URL -u $NEXUS_USR -p $NEXUS_PSW"
        }
    }
    stage('Docker build') {
        steps {
            sh 'env'
            sh "sudo docker build -t $NEXUS_HOST/${env.JOB_NAME}:${env.BUILD_NUMBER}.0 ."
        }
    }
    stage('Docker push') {
        steps {
            sh 'env'
            sh "sudo docker push $NEXUS_HOST/${env.JOB_NAME}:${env.BUILD_NUMBER}.0"
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
