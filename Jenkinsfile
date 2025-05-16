pipeline {
  agent any
  tools {
    maven 'Maven 3.8.1'   // Configure this name in Global Tool Configuration
  }
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/opstree/java-sample-app.git'
      }
    }
    stage('Build') {
      steps {
        sh 'mvn clean install'
      }
    }
    stage('Code Analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          sh 'mvn sonar:sonar -Dsonar.projectKey=java-sample-app'
        }
      }
    }
  }
}
