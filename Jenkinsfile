pipeline {
  agent any
  stages {
    
    stage('Build Docker Image') {
      steps {
          sh ' docker build -t shebl22/solar-system:$GIT_COMMIT '
      }

    }
  }
}