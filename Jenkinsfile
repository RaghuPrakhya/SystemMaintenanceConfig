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
              echo Printing all build parameter Files
              ls -l *.txt
              for t in *.txt
              do
               cat ${t}
              done
              echo Printing all environment variables
              env | sort
              echo Checking if there is a discrepancy between the build paramters displayed and in the files
              echo If there is a discrepancy request a rerun by email
           '''
      }
    }
  }
}
