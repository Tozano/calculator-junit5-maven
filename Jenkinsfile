pipeline {
  agent { docker { image 'maven:3.9-eclipse-temurin-17' } }
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
