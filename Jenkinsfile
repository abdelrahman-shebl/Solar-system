pipeline {
  agent any
  environment {
      MONGO_URI = 'mongodb+srv://supercluster.d83jj.mongodb.net/superData'
      MONGO_USERNAME = credentials('mongo-user-name')
      MONGO_PASSWORD = credentials('mongo-password')
      GITHUB_TOKEN = credentials('Github PAT')
  }
  stages {
    
    stage('Build Docker Image') {
      steps {
          sh ''' docker build -t shebl22/solar-system:$GIT_COMMIT .   
                echo "start docker" 
          '''
              
      }   
    
    }
       
    // stage('Trivy Scanner') {
    //   steps {
    //       sh '''
    //            trivy image shebl22/solar-system:$GIT_COMMIT \
    //            --severity LOW,MEDIUM,HIGH \
    //           --exit-code 0 \
    //           --quiet \
    //           --format json -o trivy-image-MEDIUM.results.json


    //           trivy image shebl22/solar-system:$GIT_COMMIT \
    //           --severity CRITICAL \
    //           --exit-code 0 \
    //           --quiet \
    //           --format json -o trivy-image-CRITICAL.results.json
    //       '''
    //   }
    //   post {
    //     always {
    //       sh '''
    //           trivy convert \
    //             --format template --template "@/usr/local/share/trivy/templates/html.tpl" \
    //             --output trivy-image-MEDIUM.results.html trivy-image-MEDIUM.results.json

    //           trivy convert \
    //             --format template --template "@/usr/local/share/trivy/templates/html.tpl" \
    //             --output trivy-image-CRITICAL.results.html trivy-image-CRITICAL.results.json


    //       '''



    //     }

    //   }

    // }

    // stage('Push to docker hub') {
    //   steps {

    //     withDockerRegistry(credentialsId: 'docker hub credentials', url: "") {
    //         sh ' docker push shebl22/solar-system:$GIT_COMMIT  '
    //     }
         
    //   } 

    // }


    stage('K8S Update Image Tag') {
      when{
          branch 'PR*'
      }
      steps {
        
       sh 'git clone https://github.com/abdelrahman-shebl/solar-system-gitops-argocd'
       dir("solar-system-gitops-argocd/kubernetes"){
        sh '''

        git checkout main 
        git checkout -b feature-$BUILD_ID
        sed -i "s#shebl22.*#shebl22/solar-system:$GIT_COMMIT#G" deployment.yml
        cat deployment.yml

        git config --global user.email "sheblabdo00@gmail.com"
        git remote set-url origin https://$GITHUB_TOKEN@github.com/abdelrahman-shebl/solar-system-gitops-argocd

        git add . 
        git commit -am "UPdated docker image"
        git push -u origin feature-$BUILD_ID
        '''
       }
         
      } 

    }

  }

  post {
      always {

      script {
        if (fileExists('solar-system-gitops-argocd')){
          sh 'rm -rf solar-system-gitops-argocd'
        }
      }



        // publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, icon: '', keepAll: true, reportDir: './', reportFiles: 'trivy-image-MEDIUM.results.html', reportName: 'trivy-image-MEDIUM.results.html', reportTitles: '', useWrapperFileDirectly: true])

        // publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, icon: '', keepAll: true, reportDir: './', reportFiles: 'trivy-image-CRITICAL.results.html', reportName: 'trivy-image-CRITICAL.results.html', reportTitles: '', useWrapperFileDirectly: true])
      }
    } 
}