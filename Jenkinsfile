pipeline {
  agent any
  stages {
    stage('Environment') {
      steps {
        sh '''
              ls -l *.txt
              for t in *.txt
              do
               cat ${t}
              done
              for x in `env`
              do
              echo $x
              done
           '''
      }
    }
  }
}
