/*
 * Orchestator Pipeline
 */

def myoutput

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

              def lastSuccessfulCommit = getLastSuccessfulCommit()
              def currentCommit = commitHashForBuild( currentBuild.rawBuild )
              if (lastSuccessfulCommit) {
                commits = sh(
                  script: "git rev-list $currentCommit \"^$lastSuccessfulCommit\"",
                  returnStdout: true
                ).split('\n')
                println "Commits are: $commits"
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

def getLastSuccessfulCommit() {
  def lastSuccessfulHash = null
  def lastSuccessfulBuild = currentBuild.rawBuild.getPreviousSuccessfulBuild()
  if ( lastSuccessfulBuild ) {
    lastSuccessfulHash = commitHashForBuild( lastSuccessfulBuild )
  }
  return lastSuccessfulHash
}

/**
 * Gets the commit hash from a Jenkins build object, if any
 */
@NonCPS
def commitHashForBuild( build ) {
  def scmAction = build?.actions.find { action -> action instanceof jenkins.scm.api.SCMRevisionAction }
  return scmAction?.revision?.hash
}



