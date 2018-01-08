pipeline { 
  agent any

  environment {
    GIT_BRANCH = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
  }
  
  stages {
    stage ('SonarQube analysis') {
      steps {
        script {
          scannerHome = tool 'SonarQube Scanner'
        }
        withSonarQubeEnv('Sonar') {
        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=cueops -Dsonar.sources=." 
        		}
          }
        }
     }
    stage('SonarQube Quality Gate') { 
      steps {
      	script {
      		qualitygate = waitForQualityGate()
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
