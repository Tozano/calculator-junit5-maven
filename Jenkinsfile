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
              configFileProvider([configFile(fileId: '8ec06d05-3ad2-4bcf-884d-b0601a08deab', variable: 'MAVEN_SETTINGS')]) {
                // Fournit les credentials Nexus
                withCredentials([usernamePassword(credentialsId: 'nexus-cred', usernameVariable: 'jenkins', passwordVariable: 'GGSKILL59!')]) {
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
