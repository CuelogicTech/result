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
    stage("SonarQube Quality Gate") { 
      steps {
      	script {
      		def qualitygate = waitForQualityGate()
      		if (qualitygate.status != "OK") {
      			echo "error Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
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
}
