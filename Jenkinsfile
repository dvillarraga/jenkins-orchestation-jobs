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
    triggers{
      cron('*/10 * * * *')
    }
    stages{
      stage("Validating Changes"){
        when{
          expression { BRANCH_NAME ==~ /^(?:(?!prod).)*$/ }
        }
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
              println "The last successful commit was: $lastSuccessfulCommit"
              commits = sh(
                script: "git rev-list $currentCommit \"^$lastSuccessfulCommit\"",
                returnStdout: true
              ).split('\n')
              println "READY"
              println "Commits are: $commits"
            } else {
              println "There were no previous successful builds"
            }
          }
        }
      }
      stage("Build Job"){
        when{
          expression { BRANCH_NAME ==~ /^(?:(?!prod).)*$/ }
        }
        steps{
          echo "#########################"
          echo "### Launching PROVISION-A"
          echo "#########################"
          build "../infrastructure/provision-a/${env.BRANCH_NAME}"
        }
      }
    }
}
