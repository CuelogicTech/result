pipeline { 
  agent any

  environment {
    GIT_BRANCH = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
  }
  
  stages {
    stage ("SonarQube analysis") {
     steps {
        script {
          	def sonarqubeScannerHome = tool 'SonarScanner';
          	}
        withEnv(["PATH=/usr/bin: ..."]) {
        withSonarQubeEnv("Sonar") {
        sh "${sonarqubeScannerHome}/bin/sonar-scanner"
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
