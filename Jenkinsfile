pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'mvnw -B -Dskiptests clean package'
        stash(name: 'war', includes: '/target/**')
      }
    }
    stage('Backend') {
      steps {
        parallel(
          "Unit test": {
            unstash 'war'
            bat 'mvnw -B test'
            junit '**/surefire-reports/**/*.xml'
            
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