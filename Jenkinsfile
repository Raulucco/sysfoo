pipeline {
  agent {
    docker {
      image 'maven:3.6.3-jdk-11-slim'
    }

  }
  stages {
    stage('build') {
      steps {
        echo 'compiling sysfoo app'
        sh 'mvn compile'
      }
    }

    stage('test') {
      steps {
        echo 'running unittest'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      steps {
        echo 'deploy'
        sh 'mvn package -DskipTests'
      }
    }

  }
  tools {
    maven 'Maven 3.6.3'
  }
  post {
    always {
      echo 'this pipeline has completed...'
    }

  }
}