pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'echo Build'
      }
    }
    stage('Backend') {
      steps {
        parallel(
          "Unit test": {
            bat 'echo unit test'
            
          },
          "Performance": {
            bat 'echo performance'
            
          }
        )
      }
    }
    stage('Frontend') {
      steps {
        bat 'echo Frontend'
      }
    }
    stage('Static Analysis') {
      steps {
        bat 'echo static analysis'
      }
    }
    stage('Deploy') {
      steps {
        bat 'echo deploy'
      }
    }
  }
}