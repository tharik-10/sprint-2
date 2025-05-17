pipeline {
  agent any

  environment {
    SNYK_TOKEN = credentials('SNYK_TOKEN')
  }

  stages {
    stage('Checkout Repositories') {
      steps {
        dir('attendance-api') {
          git url: 'https://github.com/OT-MICROSERVICES/attendance-api.git', branch: 'main'
        }
        dir('notification-worker') {
          git url: 'https://github.com/OT-MICROSERVICES/notification-worker.git', branch: 'main'
        }
      }
    }

    stage('Setup Python and Install Dependencies') {
      steps {
        dir('attendance-api') {
          sh '''
            python3 -m venv venv
            source venv/bin/activate
            pip install -r requirements.txt
            pip install flake8 pytest coverage snyk
          '''
        }
        dir('notification-worker') {
          sh '''
            python3 -m venv venv
            source venv/bin/activate
            pip install -r requirements.txt
            pip install flake8 pytest coverage snyk
          '''
        }
      }
    }

    stage('Lint Check') {
      steps {
        dir('attendance-api') {
          sh 'source venv/bin/activate && flake8 .'
        }
        dir('notification-worker') {
          sh 'source venv/bin/activate && flake8 .'
        }
      }
    }

    stage('Test & Coverage') {
      steps {
        dir('attendance-api') {
          sh 'source venv/bin/activate && pytest --cov=. --cov-report=xml'
        }
        dir('notification-worker') {
          sh 'source venv/bin/activate && pytest --cov=. --cov-report=xml'
        }
      }
    }

    stage('Snyk Vulnerability Scan') {
      steps {
        dir('attendance-api') {
          sh 'source venv/bin/activate && snyk test --file=requirements.txt'
        }
        dir('notification-worker') {
          sh 'source venv/bin/activate && snyk test --file=requirements.txt'
        }
      }
    }
  }

  post {
    success {
      slackSend(channel: '#general', message: "✅ CI Passed: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
    }
    failure {
      slackSend(channel: '#general', message: "❌ CI Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
    }
  }
}
