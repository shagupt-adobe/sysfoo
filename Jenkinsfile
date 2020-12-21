pipeline {
  agent any
  tools {
    maven 'Maven 3.6.3'
  }
  
  stages {
    stage('Build') {
      steps {
        sh 'mvn compile'
      }
    }
    
    stage('Unit Tests') {
      steps {
        sh 'mvn clean test'
      }
    }
    
    stage('Package') {
      steps {
        sh 'mvn package -DskipTests'
      }
    }
  }
}
