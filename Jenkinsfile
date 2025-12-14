pipeline {
  agent any

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

