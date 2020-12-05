pipeline {
  agent any
  stages {
    stage('Environment') {
      steps {
        sh '''
              for x in `env`
              do
              echo $x
              done
           '''
      }
    }
  }
}
