pipeline {
  agent any
  stages {
    stage('Test') {
      agent any
      environment {
        Name = 'Development'
      }
      steps {
        validateDeclarativePipeline 'Jenkinsfile'
      }
    }
    stage('Build') {
      agent {
        dockerfile {
          filename 'Dockerfile'
        }

      }
      steps {
        build 'Build Images'
      }
    }
  }
}