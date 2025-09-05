pipeline {
  agent any

  tools {
    jdk 'jdk-21'      // match Jenkins installed JDK name
    maven 'Maven'    // configure Maven tool in Jenkins with this name
  }

  options {
    timestamps()
    // ansiColor('xterm')  // uncomment after installing AnsiColor plugin
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build & Test') {
      steps {
        bat 'mvn -B -q clean install'
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

  stage('Run App') {
      when {
        expression { fileExists('target/simple-maven-project-1.0-SNAPSHOT.jar') }
      }
      steps {
        // Run your Hello World class directly from JAR
        bat 'java -cp target/simple-maven-project-1.0-SNAPSHOT.jar com.example.App'
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
