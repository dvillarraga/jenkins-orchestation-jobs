/*
 * Orchestator Pipeline
 */

@Library('my-shared-library')

def myoutput
def lastSuccessfulCommit = buildscm.getLastSuccessfulCommit()
def currentCommit = buildscm.commitHashForBuild( currentBuild.rawBuild )
def commits

pipeline{
    agent any
    stages{
        stage("Validating Changes"){
            steps{
              script{
                myoutput = sh (
                  script: """
                  #!/bin/bash
                  echo $PWD
                  echo \$(ls -lsa)
                  """,
                  encoding: "UTF-8",
                  returnStdout: true
                )
              }
              echo "The test output is ${myoutput}"
              script{
                if (lastSuccessfulCommit) {
                  commits = sh(
                    script: "git rev-list $currentCommit \"^$lastSuccessfulCommit\"",
                    returnStdout: true
                  ).split('\n')
                  println "change one"
                  println "Commits are: $commits"
                }
              }
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
