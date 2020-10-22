/*
 * Orchestator Pipeline
 */

def myoutput

pipeline{
    agent any
    stages{
        stage("Validating Changes"){
            steps{
                echo "======== Validating changes in Infrastructure ========"
                myoutput = sh (
                  script: """
                  #!/bin/bash
                  echo $PWD
                  echo \$(ls -lsa)
                  """
                  encoding: "UTF-8"
                  returnStdout: true
                )
                echo "The test output is ${myoutput}"
            }
        }
    }
    post{
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}