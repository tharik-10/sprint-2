pipeline {
  agent any
  tools {
    maven 'Maven 3.9.2'   // Configure this name in Global Tool Configuration
  }
  stages {
    stage('Checkout') {
      steps {
        git ' https://github.com/jenkins-docs/simple-java-maven-app'
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
