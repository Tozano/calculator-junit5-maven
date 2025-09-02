pipeline {
    agent {
        docker {
            // Docker CLI + JDK + Maven
            image 'maven:3.9.6-eclipse-temurin-17' 
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

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
