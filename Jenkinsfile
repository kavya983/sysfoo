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

  script {
      if(env.BRANCH_NAME == "dockeragent") {
        stage('Run publish') {
            parallel {
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
                  agent any
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
      } else {
        println "skipping"
      }
    }
  }
}
