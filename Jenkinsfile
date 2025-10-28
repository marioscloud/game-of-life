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
        sh 'mvn -B -DskipTests clean install'
      }
    }
  }

  post {
    always {
      junit '**/target/surefire-reports/*.xml'
      archiveArtifacts artifacts: '**/target/*.jar, **/target/*.war', allowEmptyArchive: true
    }
  }
}