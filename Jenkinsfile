pipeline {
  agent any
  stages {
    stage('sleep') {
      steps {
        sleep 10
      }
    }

  }
  environment {
    test = 'test-value'
  }
  parameters {
    string(defaultValue: 'maintainer', description: 'Enter user role:', name: 'userRole')
  }
}