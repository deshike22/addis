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
    stage('Stage in UAT') {
      agent {
        docker {
          image 'addis'
        }

      }
      steps {
        node(label: 'Create pod') {
          tool 'kubectl'
        }

      }
    }
  }
}