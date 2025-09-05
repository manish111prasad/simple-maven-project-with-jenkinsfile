pipeline {
  agent any

  tools {
    // Match these names with Jenkins -> Manage Jenkins -> Tools installations
    jdk 'JDK17'
    maven 'Maven3'
  }

  options {
    timestamps()
    ansiColor('xterm')
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build & Test') {
      steps {
        sh label: 'Maven build', script: 'mvn -B -q clean install'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }

    stage('Archive Artifact') {
      when {
        expression { fileExists('target') }
      }
      steps {
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true, onlyIfSuccessful: true
      }
    }
  }

  post {
    failure {
      echo 'Build failed. Check console output and test reports.'
    }
    success {
      echo 'Build succeeded! Artifact archived.'
    }
    always {
      cleanWs()
    }
  }
}
