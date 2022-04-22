pipeline {
  agent any
  stages {
    stage('package') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      post {
        always {
          echo 'This pipeline is completed..'
          archiveArtifacts(artifacts: 'target/*.war', followSymlinks: false)
        }

      }
      steps {
        echo 'packaging'
        sh 'mvn package -DskipTests'
        echo 'done'
      }
    }

    stage('Docker push') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
            def dockerImage = docker.build("kavya333/sysfoo:v${env.BUILD_ID}", "./")
            dockerImage.push()
            dockerImage.push("latest")
            dockerImage.push("dev")
          }
        }

      }
    }

  }
}