pipeline {
  agent {
    docker {
      image 'maven:3.6.3-jdk-11-slim'
    }

  }
  stages {
    stage('build') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'compiling sysfoo app'
        sh 'mvn compile'
      }
    }

    stage('test') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'running unittest'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        when {
           branch 'master'
        }

        echo 'deploy'
        sh 'mvn package -DskipTests'
        archiveArtifacts 'target/*.war'
      }
    }

    stage('Docker build and publish') {
      steps {

        when {
         branch 'master'
        }
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {

            def dockerImage = docker.build("raulcm80/sysfoo:v${env.BUILD_ID}", "./")

            dockerImage.push()

            dockerImage.push("latest")

          }
        }

      }

      stage('deploy to dev') {
        when {
            beforeAgent true
            branch 'master'
        }

        agent any

        steps {
            echo 'Deploying with docker compose'
            sh 'docker-compose up -d'
        }
      }
    }

  }
  tools {
    maven 'Maven 3.6.3'
  }
  environment {
    Maven = '3.6.3'
  }
  post {
    always {
      echo 'this pipeline has completed...'
    }

  }
}