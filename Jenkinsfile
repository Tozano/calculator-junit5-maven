pipeline {
  agent any
  options { timestamps() }

  stages {
    stage('Build & Test') {
      steps {
        sh 'mvn -B clean test package'
      }
    }
  }
  post {
    always {
      junit 'target/surefire-reports/*.xml'
      archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
    }
  }
}
