pipeline {
  agent none
  stages {
    stage('Build') {
	  when {
		anyOf {
		  branch 'master';
		  changeRequest()
		}
	  }
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        sh 'mvn compile'
      }
    }

    stage('Unit Tests') {
	  when {
		anyOf {
		  branch 'master';
		  changeRequest()
		}
	  }
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        sh 'mvn clean test'
      }
    }

    stage('Package') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      when {
        branch 'master'
      }
      steps {
        sh 'mvn package -DskipTests'
        archiveArtifacts 'target/*.war'
      }
    }

    stage('Docker Build & Publish') {
      agent any
      when {
        branch 'master'
      }
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
            def dockerImage = docker.build("shankyrox/sysfoo:v${env.BUILD_ID}", "./")
            dockerImage.push()
            dockerImage.push("latest")
            dockerImage.push("dev")
          }
        }

      }
    }

    stage('Docker compose') {
      agent any
	  when { branch 'master' }
      steps {
        sh 'docker-compose up -d '
      }
    }

  }
  tools {
    maven 'Maven 3.6.3'
  }
}
