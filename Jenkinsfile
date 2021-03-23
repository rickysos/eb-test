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
      stage('Container Scanning') {
         parallel {
            stage('Run Anchore') {
               steps {
                  bash """
                    echo "I is Ancore" 
                  """)
                  anchore bailOnFail: false, bailOnPluginFail: false, name: 'anchore_images'
               }
            }
            stage('Run Trivy') {
               steps {
                 bash """
                 docker build -t jenkins-pipeline .
                 docker images -a
                 """
               }
            }
         }
      }
   }
}

