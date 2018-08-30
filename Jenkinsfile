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
  }
}