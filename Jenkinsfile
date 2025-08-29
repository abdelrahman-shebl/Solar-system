pipeline {
  agent any
  stages {
    stage('parallel') {
      parallel{

          stage('echo name') {
            steps {
              sh ' echo "hello world" '   
        }
      }

      stage('echo other') {
            steps {
              sh ' echo "hello world" '   
        }
      }

      }
    }
    stage('npm insatll') {
      steps {
        sh ' npm install --no-audit '   
      }
    }

  }
}