pipeline {
  agent any
  parameters{ 
    string(defaultValue: "maintainer", 
      description: 'Enter user role:', name: 'userRole')
  }
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
}
