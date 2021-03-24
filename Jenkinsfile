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
''') {
    node(POD_LABEL) {
      stage('Echo Things') {
        git 'https://github.com/rickysos/eb-test.git'
        sh 'ls -ltrah'
      }
      stage('Build Docker image') {
        container('docker') {
            sh 'docker version && docker build -t testing .'
        }
      }
    }
}
