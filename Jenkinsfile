pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        // Use -B for batch mode, skip tests to speed up CI if you prefer
        sh 'mvn -B -DskipTests clean install'
      }
    }
  }

  post {
    always {
      // Adjust paths if your tests/report locations differ
      junit '**/target/surefire-reports/*.xml'
      archiveArtifacts artifacts: '**/target/*.jar, **/target/*.war', allowEmptyArchive: true
    }
  }
}
