pipeline { 
  agent any

  environment {
    DOCKERHUB = credentials('dockerhub')
    GIT_BRANCH = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
  }
  
  stages {
    stage ('Notify CI start') {
      steps {
        sh "curl -k http://${env.ST2_URL}/api/v1/webhooks/codecommit -d '{\"name\": \"${env.JOB_NAME}\", \"build\": {\"branch\": \"${env.GIT_BRANCH}\", \"phase\": \"STARTED\", \"number\": \"${env.BUILD_ID}\"}}' -H 'Content-Type: application/json' -H 'st2-api-key: ${env.ST2_API_KEY}'"
      }
    }
//    stage ('Checkout Code') {
//      steps {
//        sh "export GIT_TRACE=1"
//        checkout scm
//        sh "git config --global core.compression 0"
//        checkout scm: [$class: 'GitSCM', extensions: [[$class: 'CheckoutOption', timeout: 240, shallow: true]]]
//      }
//    }
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
          sh "sudo docker build -t ${env.DOCKERHUB_REPO}/${env.JOB_NAME}:${env.GIT_BRANCH}-${env.BUILD_NUMBER} ."
        }
    }
    stage('Docker push') {
        steps {
            sh 'env'
          sh "sudo docker push ${env.DOCKERHUB_REPO}/${env.JOB_NAME}:${env.GIT_BRANCH}-${env.BUILD_NUMBER}"
            sh "curl -k http://${env.ST2_URL}/api/v1/webhooks/codecommit -d '{\"name\": \"${env.JOB_NAME}\", \"build\": {\"branch\": \"${env.GIT_BRANCH}\", \"status\": \"SUCCESS\", \"number\": \"${env.BUILD_ID}\"}}' -H 'Content-Type: application/json' -H 'st2-api-key: ${env.ST2_API_KEY}'"
        }
    }
  }
  post {
        always {
            echo 'Cleaning the workspace & docker image'
          sh "sudo docker rmi ${env.DOCKERHUB_REPO}/${env.JOB_NAME}:${env.GIT_BRANCH}-${env.BUILD_NUMBER}"
            deleteDir() /* clean up our workspace */
     }
  }
}
