pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build Jar') {
      steps {
        sh 'mvn -B -U clean package || ./mvnw -B -U clean package'
      }
    }

    stage('Deploy to Nexus') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'nexus-creds', usernameVariable: 'NX_USER', passwordVariable: 'NX_PASS')]) {
          sh '''
            mkdir -p ~/.m2
            cat > ~/.m2/settings.xml <<EOF
<settings>
  <servers>
    <server>
      <id>nexus</id>
      <username>${NX_USER}</username>
      <password>${NX_PASS}</password>
    </server>
  </servers>
</settings>
EOF
            mvn -B -U deploy || ./mvnw -B -U deploy
          '''
        }
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
    }
  }
}

