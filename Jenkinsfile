pipeline { 
  agent any

  environment {
    DOCKERHUB = credentials('dockerhub')
    GIT_BRANCH = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
  }
  
  stages {
    stage ('Checkout Code') {
      steps {
        checkout scm
      }
    }
    stage ('Build app') {
      steps {
        sh "echo Add build commands here"
      }
    }
    stage('Dockerhub login') {
        steps {
            sh "sudo docker login -u $DOCKERHUB_USR -p $DOCKERHUB_PSW"
        }
    }
    stage('Docker build') {
        steps {
            sh 'env'
            sh "sudo docker build -t cueops/${env.JOB_NAME}:${env.GIT_BRANCH}-${env.BUILD_NUMBER} ."
        }
    }
    stage('Docker push') {
        steps {
            sh 'env'
            sh "sudo docker push cueops/${env.JOB_NAME}:${env.GIT_BRANCH}-${env.BUILD_NUMBER}"
            sh "curl -k http://34.200.248.216/api/v1/webhooks/codecommit -d '{"name": ${env.JOB_NAME}, "build": { "branch":${env.GIT_BRANCH}, "number":${env.BUILD_NUMBER}, "status" :"SUCCESS"} }' -H 'Content-Type: application/json' -H 'st2-api-key: OGVhYWExZDU1NDg3NDZmZGMyOTY5MTgxMDNjNzNkNjEzZTFlY2E4YTIxNTViOWYwMmZhZWM1NTgwYTE0Zjc3YQ'"
        }
    }
  }
}
