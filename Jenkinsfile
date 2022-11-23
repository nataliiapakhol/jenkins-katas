pipeline {
  agent any
  stages {
    stage('Clone down') {
      agent {
        label 'master-label'
      }
      steps {
        stash excludes: '.git/', name: 'code'
      }
    }
    stage('Parallel execution') {
      parallel {
        stage('Say Hello') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('build app') {
          agent {
            docker {
              image 'gradle:6-jdk11'
            }
          }
          options {
            skipDefaultCheckout true
          }
          steps {
            unstash 'code'  
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
            sh 'ls -la'
            deleteDir()
            sh 'ls -la'
          }
        }

      }
    }

  }
}
