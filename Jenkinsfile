/**
 * https://github.com/GoogleContainerTools/kaniko
 * https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-secret-in-the-cluster-that-holds-your-authorization-token


def label = "kaniko-${UUID.randomUUID().toString()}"

podTemplate(name: 'kaniko', label: label, yaml: """
kind: Pod
metadata:
  name: kaniko
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: Always
    command:
    - /busybox/cat
    tty: true
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /root
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: regcred
          items:
            - key: .dockerconfigjson
              path: .docker/config.json
"""
  ) {

  node(label) {
    stage('Build with Kaniko') {
      git branch: 'Development', url: 'https://github.com/deshike22/addis.git'
      container(name: 'kaniko', shell: '/busybox/sh') {
        withEnv(['PATH+EXTRA=/busybox']) {
          sh '''#!/busybox/sh
          /kaniko/executor -f `pwd`/Dockerfile -c `pwd` --skip-tls-verify --destination=bimehta/addis:latest
          '''
        }
      }
    }
  }
}
*/
def podlabel = "kaniko-${UUID.randomUUID().toString()}"

pipeline {
  
  agent {
    kubernetes {
      label podlabel
      yamlFile 'kaniko.yaml'
    }
  }
  stages {
    stage('Build with Kaniko'){
      steps {
        git branch: 'Development', url: 'https://github.com/deshike22/addis.git'
        container(name: 'kaniko', shell: '/busybox/sh') {
          withEnv(['PATH+EXTRA=/busybox']) {
            sh '''#!/busybox/sh
            /kaniko/executor -f `pwd`/Dockerfile -c `pwd` --skip-tls-verify --destination=bimehta/addis:latest
            '''
          }
        }
      }
    }

    stage('Deploy to Development'){
      steps {
        container(name: 'myapp') {
          sh 'echo Hello world!'
        }
      }
    }
  }
}