pipeline {
  agent any
  stages {
    stage('Environment') {
      steps {
        echo " The System is ${params.SystemName}"
        echo " The Locations are ${params.Locations}"
        echo " The SOPS are ${params.SOPS}"        
        echo " The Frequency is ${params.Frequency}"
        echo " The EmailIds are ${params.EmailIds}"
        sh '''
              ls -l *.txt
              for t in *.txt
              do
               cat ${t}
              done
              env
              for x in `env`
              do
              echo $x
              done
           '''
      }
    }
  }
}
