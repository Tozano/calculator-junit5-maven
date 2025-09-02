pipeline {
  agent any
  options { timestamps() }
  
  environment {
    NEXUS_URL = 'http://nexus:8081' // URL interne via réseau Docker 'ci'
  }

  stages {
    stage('Build & Test') {
      steps {
        script {
          sh 'mvn -B clean test package'
        }
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
      }
    }
    stage('Deploy to Nexus') {
      steps {
        script {
          // Utilise Maven installé dans Jenkins
            dir('/workspace/calculator') {
              // Fournit le settings.xml configuré dans Jenkins (Config File Provider)
              configFileProvider([configFile(fileId: 'maven-settings-nexus', variable: 'MAVEN_SETTINGS')]) {
                // Fournit les credentials Nexus
                withCredentials([usernamePassword(credentialsId: 'nexus-cred', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PWD')]) {
                  // Déploiement
                  sh 'mvn -B -s "$MAVEN_SETTINGS" -DskipTests deploy'
                }
              }
          }
        }
      }
    }
  }
}
