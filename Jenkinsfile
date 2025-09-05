pipeline {                        // OPEN pipeline
  agent any

  tools {
    jdk 'jdk-21'
    maven 'Maven'
  }

  options {
    timestamps()
    // ansiColor('xterm')
  }

  stages {                        // OPEN stages
    stage('Checkout') {           // OPEN stage Checkout
      steps {
        checkout scm
      }
    }                             // CLOSE stage Checkout

    stage('Build & Test') {       // OPEN stage Build & Test
      steps {
        bat 'mvn -B -q clean install'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }                             // CLOSE stage Build & Test

    stage('Archive Artifact') {   // OPEN stage Archive Artifact
      when {
        expression { fileExists('target') }
      }
      steps {
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true, onlyIfSuccessful: true
      }
    }                             // CLOSE stage Archive Artifact

    stage('Run App') {            // OPEN stage Run App
      when {
        expression { fileExists('target/simple-maven-project-1.0-SNAPSHOT.jar') }
      }
      steps {
        bat 'java -cp target/simple-maven-project-1.0-SNAPSHOT.jar com.example.App'
      }
    }                             // CLOSE stage Run App
  }                               // CLOSE stages

  post {                          // OPEN post
    failure {
      echo 'Build failed. Check console output and test reports.'
    }
    success {
      echo 'Build succeeded! Artifact archived.'
    }
    always {
      cleanWs()
    }
  }                               // CLOSE post
}                                 // CLOSE pipeline
