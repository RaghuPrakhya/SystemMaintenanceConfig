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
              echo I am running $SHELL
              echo which returns `which sh`
              echo Printing all build parameter Files
              ls -l *.txt
              for t in *.txt
              do
               cat ${t}
              done
              
              echo Printing all environment variables
              env | sort
              
              echo Validating parameter files and parameters passed
              SystemNamesPath=${WORKSPACE}/Systems.txt
              LocationsPath=${WORKSPACE}/Locations.txt
              SOPSPath=${WORKSPACE}/SOPS.txt
              FrequencyPath=${WORKSPACE}/Frequency.txt
              DLPath=${WORKSPACE}/PlatformDL.txt
              
              paths=( "${SystemNamesPath}" "${LocationsPath}" "${SOPSPath}" "${FrequencyPath}" "${DLPath}" )
              parms=( ${params.SystemName}" ${params.Locations}" ${params.SOPS}" ${params.Frequency}" ${params.EmailIds}" )
              for i in ${!paths[@]}
              do

                if [ ! -f ${paths[$i]} ]
                then
                  error("${paths[$i]} should exist failing the job")                               
                fi
                
                totCnt=`grep -v "^[ \t]*$" ${paths[$i]} | wc -l`
                if [ $totCnt -eq 0 ]
                then
                  error("${paths[$i]} should not be empty failing the job")                               
                fi                

                matchCnt=`grep "${parms[$i]}" ${paths[$i]} | wc -l`
                if [ $matchCnt -ne 1 ]
                then
                  error("There should be an unique match for ${parms[$i]} in ${paths[$i]}")                               
                fi 
              done
              
              #echo Checking if there is a discrepancy between the build paramters displayed and in the files
              #echo If there is a discrepancy request a rerun by email
           '''
      }
    }
  }
}
