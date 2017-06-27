pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'mvn -B -DskipTests clean package'
        stash(name: 'war', includes: '/target/**')
      }
    }
    stage('Backend') {
      steps {
        parallel(
          "Unit test": {
            unstash 'war'
            bat 'mvn -B test'
            junit '**/surefire-reports/**/*.xml'
            
          },
          "Performance": {
            unstash 'war'
            bat 'mvn -B gatling:execute'
            
          }
        )
      }
    }
    stage('Frontend') {
      agent { 
        docker 'node:alpine'
      }
      steps {
        bat 'yarn install'
        bat 'yarn global add gulp-cli'
        bat 'gulp test'
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