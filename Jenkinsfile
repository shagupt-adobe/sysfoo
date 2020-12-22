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
      when { branch 'master' }
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        sh 'mvn package -DskipTests'
        archiveArtifacts 'target/*.war'
      }
    }

    stage('Docker Build & Publish') {
      when { branch 'master' }
      agent any
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

  }
  tools {
    maven 'Maven 3.6.3'
  }
}
