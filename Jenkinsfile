/**
 * https://github.com/GoogleContainerTools/kaniko
 * https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-secret-in-the-cluster-that-holds-your-authorization-token

*/

def podlabel = "${UUID.randomUUID().toString()}"

pipeline {
  stages {
    stage('Build with Kaniko'){
      agent {
        kubernetes {
          label 'kaniko-'podlabel
          yamlFile 'kaniko.yaml'
        }
      }
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
      agent {
        kubernetes {
          label 'myapp-'podlabel
          yamlFile 'myapp.yaml'
        }
      }      
      steps {
        container(name: 'myapp') {
          sh 'echo Hello world!'
        }
      }
    }
  }
}