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
        sh '''#!/bin/bash        
              echo Printing all build parameter Files
              ls -l *.txt
              for t in *.txt
              do
               cat ${t}
              done
              
              #echo Printing all environment variables
              #env | sort
              
              echo Validating parameter files and parameters passed
              SystemNamesPath=${WORKSPACE}/Systems.txt
              LocationsPath=${WORKSPACE}/Locations.txt
              SOPSPath=${WORKSPACE}/SOPS.txt
              FrequencyPath=${WORKSPACE}/Frequency.txt
              DLPath=${WORKSPACE}/PlatformDL.txt
              
              echo $SystemNamesPath
              echo $LocationsPath 
              echo $SOPSPath 
              echo $FrequencyPath 
              echo $DLPath 
              
              #OIFS=$IFS
              #IFS=","
              paths=(${SystemNamesPath} ${LocationsPath} ${SOPSPath} ${FrequencyPath} ${DLPath})
              parms=("${SystemName}" "${Locations}" "${SOPS}" "${Frequency}" "${EmailIds}")
              for i in ${!paths[@]}
              do
                echo ${paths[$i]} 
                echo ${parms[$i]}

              done
              
              #echo Checking if there is a discrepancy between the build paramters displayed and in the files
              #echo If there is a discrepancy request a rerun by email
           '''
      }
    }
  }
}
