pipeline {
   environment { 
       registry = "rickysos/eb-test" 
       registryCredential = 'dockerhub_id' 
       dockerImage = '' 
   }
   agent any
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

