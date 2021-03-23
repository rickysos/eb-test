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
            pwsh(script: 'docker images -a')
            pwsh(script: """
               docker images -a
               docker build -t jenkins-pipeline .
               docker images -a
            """)
         }
      }
      stage('Container Scanning') {
         parallel {
            stage('Run Anchore') {
               steps {
                  pwsh(script: """
                     Write-Output "blackdentech/jenkins-course" > anchore_images
                  """)
                  anchore bailOnFail: false, bailOnPluginFail: false, name: 'anchore_images'
               }
            }
            stage('Run Trivy') {
               steps {
                  sleep(time: 30, unit: 'SECONDS')
                  // pwsh(script: """
                  // C:\\Windows\\System32\\wsl.exe -- sudo trivy blackdentech/jenkins-course
                  // """)
               }
            }
         }
      }
   }
}

