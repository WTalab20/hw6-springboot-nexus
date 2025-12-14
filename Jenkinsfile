pipeline {
  agent any

  stage('Debug Net') {
  steps {
    sh 'echo "HOSTNAME=$(hostname)"'
    sh 'cat /etc/resolv.conf || true'
    sh 'getent hosts github.com || true'
    sh 'curl -I https://github.com | head -n 5 || true'
    sh 'git --version || true'
  }
}

  tools {
    maven 'Maven'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh 'mvn -B clean package -DskipTests'
      }
    }

    stage('Deploy to Nexus') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'nexus-creds',
          usernameVariable: 'NEXUS_USER',
          passwordVariable: 'NEXUS_PASS'
        )]) {
          sh '''
            mvn -B deploy -DskipTests \
              -Dnexus.username=$NEXUS_USER \
              -Dnexus.password=$NEXUS_PASS
          '''
        }
      }
    }
  }
}

