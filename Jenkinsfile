def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(label: 'flask-build', yaml: """
apiVersion: v1
kind: Pod
metadata:
  name: dockerbuild
spec:
  serviceAccountName: jenkins
  containers:
    - name: helm
      image: alpine/helm:2.13.1
      command:
      - cat
      tty: true
    - name: docker
      image: docker
      command:
      - cat
      tty: true
      env:
      - name: POD_IP
        valueFrom:
          fieldRef:
            fieldPath: status.podIP
      - name: DOCKER_HOST
        value: tcp://localhost:2375
    - name: dind
      image: docker:18.05-dind
      securityContext:
        privileged: true
      volumeMounts:
        - name: dind-storage
          mountPath: /var/lib/docker
  volumes:
    - name: dind-storage
      emptyDir: {}
"""
) {
    
   pipeline {
       agent {
          docker {
              image 'node:6-alpine' 
              args '-p 3000:3000' 
          }
       }
       stages {
          stage('Install dependencies') { 
              steps {
                  sh 'npm install' 
              }
          }
          stage('Test app') {
              steps {
                  sh 'npm run test src/App.test.js'
              }
          }
          stage('Build app') {
              steps {
                  sh 'npm run build'
              }
          }
       }
   }
  
  }
