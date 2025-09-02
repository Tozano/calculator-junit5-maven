pipeline {
  agent any

  stages {
    stage('Build & Test') {
      steps {
        sh 'mvn -B clean test package' 
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t calculator-app:latest .'
      }
    }
    stage('Run Container') {
      steps {
        sh 'docker run -d --name calc -p 8081:8080 calculator-app:latest || true'
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
