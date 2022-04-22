pipeline {
  agent none
  stages {
    stage('build') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'compiling'
        sh 'mvn compile'
      }
    }

    stage('test') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'testing'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'packaging'
        sh 'mvn package -DskipTests'
        echo 'done'
      }
    }
  }
  post {
    always {
      echo 'This pipeline is completed..'
      archiveArtifacts(artifacts: 'target/*.war', followSymlinks: false)
    }
  }
}
