pipeline {
   environment { 
       registry = "rickysos/eb-test" 
       registryCredential = 'dockerhub_id' 
       dockerImage = 'docker:18.09.7-dind'
       DOCKER_TLS_CERTDIR: '' 
   }
   agent {
      docker { image 'docker:18.09.7-dind' }
   }
   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker Build') {
         steps {
             script { 
                 dockerImage = docker.build registry + ":$BUILD_NUMBER" 
             }
         }
      }
      stage('Deploy our image') { 
          steps { 
              script { 
                  docker.withRegistry( '', registryCredential ) { 
                      dockerImage.push() 
                  }
              } 
          }
      }
   }
}

