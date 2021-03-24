podTemplate(yaml: '''
apiVersion: v1
kind: Pod
spec:
  volumes:
  - name: docker-socket
    emptyDir: {}
  containers:
  - name: docker
    image: docker:19.03.1
    command:
    - sleep
    args:
    - 99d
    volumeMounts:
    - name: docker-socket
      mountPath: /var/run
  - name: docker-daemon
    image: docker:19.03.1-dind
    securityContext:
      privileged: true
    volumeMounts:
    - name: docker-socket
      mountPath: /var/run
  - name: trivy
    image: docker.io/aquasec/trivy
    tty: true
    command: ["/bin/sh"]
    securityContext:
      privileged: true
    volumeMounts:
    - name: docker-socket
      mountPath: /var/run
''') {
    node(POD_LABEL) {
      stage('Echo Things') {
        git branch: 'main', url: 'https://github.com/rickysos/eb-test.git'
        sh 'ls -ltrah'
      }
      stage('Build Docker image') {
        container('docker') {
            sh 'docker version && docker build -t testing .'
        }
      }
      stage ('Trivy Scan') {
        container(name: 'trivy', shell: '/bin/sh') {
          sh '''#!/bin/sh
             trivy testing
             # Trivy scan result processing
             my_exit_code=$?
             echo "RESULT 1:--- $my_exit_code"

             # Check scan results
             if [ ${my_exit_code} == 1 ]; then
                 echo "Image scanning failed. Some vulnerabilities found"
                 exit 1;
             else
                 echo "Image is scanned Successfully. No vulnerabilities found"
             fi;
           '''
        } 
      }
    }
}
