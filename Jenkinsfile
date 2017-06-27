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
      steps {
        bat 'mvn com.github.eirslett:frontend-maven-plugin:install-node-and-yarn -DnodeVersion=v6.9.4 -DyarnVersion=v0.19.1'
        bat 'mvn com.github.eirslett:frontend-maven-plugin:yarn'
        bat 'mvn com.github.eirslett:frontend-maven-plugin:gulp -Dfrontend.gulp.arguments=test'
        junit '**/target/test-results/karma/TESTS-*.xml'
      }
    }
    stage('Static Analysis') {
      steps {
        bat 'mvn checkstyle:checkstyle'
        checkstyle(pattern: '**/checkstyle-result.xml')
      }
    }
    stage('Deploy') {
      steps {
        bat 'echo deploy'
      }
    }
  }
}