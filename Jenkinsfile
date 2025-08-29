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

      stage('echo another ') { 
            steps {  
              sh ' echo "thank you" '   
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