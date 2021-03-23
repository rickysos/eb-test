pipeline {
   agent any

   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker Build') {
         steps {
           bash """
           docker images -a
           docker build -t jenkins-pipeline .
           docker images -a
           """
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

