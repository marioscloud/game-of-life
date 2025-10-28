pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Diagnostics (temporary)') {
      steps {
        // show workspace and poms to debug why core module is not built
        sh 'pwd; ls -la; echo "--- root pom ---"; sed -n "1,200p" pom.xml'
        sh 'echo "--- core pom ---"; sed -n "1,200p" gameoflife-core/pom.xml || true'
        sh 'echo "--- web pom ---"; sed -n "1,200p" gameoflife-web/pom.xml || true'
        sh 'echo "--- Reactor build (focused) ---"'
        sh 'mvn -B -DskipTests -pl gameoflife-web -am -X clean install | tee maven-debug.log'
        sh 'ls -la gameoflife-core/target || true'
      }
    }

    stage('Build') {
      steps {
        // Build the web module and its module dependencies in the reactor
        sh 'mvn -B -DskipTests -pl gameoflife-web -am clean install'
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