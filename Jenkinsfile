pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'compiling'
        sh 'mvn compile'
      }
    }

    stage('test') {
      steps {
        echo 'testing'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      steps {
        echo 'packaging'
        sh 'mvn package -DskipTests'
        echo 'done'
      }
    }

  }
  tools {
    maven 'Maven 3.6.3'
  }
  post {
    always {
      echo 'This pipeline is completed..'
      archiveArtifacts(artifacts: 'target/*.war', followSymlinks: false)
    }

  }
}