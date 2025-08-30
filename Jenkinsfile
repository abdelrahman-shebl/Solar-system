pipeline {
  agent any
  stages {
    
    // stage('Build Docker Image') {
    //   steps {
    //       sh ' docker build -t shebl22/solar-system:$GIT_COMMIT . '
    //   }

    // }
    
    stage('Trivy Scanner') {
      steps {
          sh '''
               trivy image shebl22/solar-system:$GIT_COMMIT \
               --severity LOW,MEDIUM \
              --exit-code 0 \
              --quiet \
              --format json -o trivy-image-MEDIUM.results.json


              trivy image shebl22/solar-system:$GIT_COMMIT \
              --severity HIGH,CRITICAL \
              --exit-code 1 \
              --quiet \
              --format json -o trivy-image-CRITICAL.results.json
          '''
      }
      post {
        always {
          sh '''
              trivy convert \
                --format template --template "@/usr/local/share/trivy/templates/html.tpl" \
                --output trivy-image-MEDIUM.results.html trivy-image-MEDIUM.results.json

              trivy convert \
                --format template --template "@/usr/local/share/trivy/templates/html.tpl" \
                --output trivy-image-CRITICAL.results.html trivy-image-CRITICAL.results.json


          '''



        }

      }

    }

    

  }

  post {
      always {
        publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, icon: '', keepAll: true, reportDir: './', reportFiles: 'trivy-image-MEDIUM.results.html', reportName: 'trivy-image-MEDIUM.results.html', reportTitles: '', useWrapperFileDirectly: true])

        publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, icon: '', keepAll: true, reportDir: './', reportFiles: 'trivy-image-CRITICAL.results.html', reportName: 'trivy-image-CRITICAL.results.html', reportTitles: '', useWrapperFileDirectly: true])
      }
    } 
}