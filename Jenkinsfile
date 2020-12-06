pipeline {
  agent any
  stages {
    stage('Validation') {
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
              
              paths=(${SystemNamesPath} ${LocationsPath} ${SOPSPath} ${FrequencyPath} ${DLPath})
              parms=("${SystemName}" "${Locations}" "${SOPS}" "${Frequency}" "${EmailIds}")
              for i in ${!paths[@]}
              do
                echo Checking if file ${paths[$i]} exists
                if [ ! -f ${paths[$i]} ]
                then
                  echo "${paths[$i]} should exist failing the job" 
                  exit 1              
                fi
                
                echo Ensuring file ${paths[$i]} is not empty
                totCnt=`grep -v "^[ \t]*$" ${paths[$i]} | wc -l`
                if [ $totCnt -eq 0 ]
                then
                  echo "${paths[$i]} should not be empty failing the job"                               
                  exit 1  
                fi                

                BKP_IFS=${IFS}
                IFS=","
                parm_list=(${parm[$i]})
                IFS=${BKP_IFS}
                for j in ${!parm_list[@]}
                do 
                  echo "Checking whther there is an unique match for ${parm_list[$j]} in ${paths[$i]}
                  matchCnt=`grep "${parm_list[$j]}" ${paths[$i]} | wc -l`
                  if [ $matchCnt -ne 1 ]
                  then
                    echo "There should be an unique match for ${parm_list[$j]} in ${paths[$i]}"                              
                    exit 1  
                  fi 
                done
                
              done
              
              #echo Checking if there is a discrepancy between the build paramters displayed and in the files
              #echo If there is a discrepancy request a rerun by email
           '''
      }
      post {
            failure {
                     script {
                             echo "Validation stage failure"
                             currentBuild.result = 'FAILURE'
                             notifyBuild(currentBuild.result)
                            }           
                    }
           }
    }
  }
}
// Function to  send notification email
def notifyBuild(String buildStatus = 'STARTED') {
    buildStatus =  buildStatus ?: 'SUCCESSFUL'
    echo Job Status is $buildStatus
    //emailext (
    //    to: env.EMAIL_RECIPIENT,
    //    from: 'no-reply@cognizant.com',
    //    subject: "Jenkins: '${env.JOB_NAME} [#${env.BUILD_NUMBER}] - $buildStatus'",
    //    body: """
    //    Jenkins Job ${env.JOB_NAME} [#${env.BUILD_NUMBER}] - $buildStatus
    //    Check console output at ${env.BUILD_URL}
    //    """
    //)  
}
