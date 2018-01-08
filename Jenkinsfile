pipeline { 
  agent any

  environment {
    GIT_BRANCH = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
  }
  
  stages {
    stage ('SonarQube analysis') {
      steps {
        script {
          scannerHome = tool 'SonarQube Scanner 2.8'
        }
        withSonarQubeEnv('SonarQube Scanner') {
        sh "${scannerHome}/bin/sonar-scanner"
        		}
          }
        }
      }
    }
  post {
        always {
            echo 'Cleaning the workspace & docker image'
            deleteDir() /* clean up our workspace */
     }
  }
