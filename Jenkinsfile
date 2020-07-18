import org.jenkinsci.plugins.workflow.steps.FlowInterruptedException

stage('root') {
  try {
    stage('win2012r2') {
      node('nomaster') {
         sleep 1
         executeKitchenTarget('win2012r2', 'windows')
      }
    }
  } catch (e) {
    currentBuild.result = 'FAILURE'
    notifyFailed()
    throw e
  } finally {
    println 'currentBuild.result: ' + currentBuild.result
    if (currentBuild.result != 'FAILURE') {
      notifySuccessful()
    }
  }
}

def executeKitchenTarget(String label, String target) {
  checkout([$class: 'GitSCM', branches: [[name: env.BRANCH_NAME]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/jpmat296/ansible.localization']]])
  stage('molecule') {
    timestamps {
      try {
        sh """#!/bin/bash
          set -x
          bash ~/bin/vms_destroy.sh
          molecule test -s $target
        """
      } catch (FlowInterruptedException ie) {
        sh """#!/bin/bash
          set -x
          win2012r2 destroy -s $target
        """
      } finally {
//         sh """#!/bin/bash
//           set -x
//           cd .kitchen
//           zip -r ../kitchen-$label-logs.zip logs
//         """
//         archiveArtifacts "kitchen-$label-logs.zip"
      }
    }
  }
}

def notifySuccessful() {
  emailext (
      subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",

body: """<p>SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at "<a href="${env.BUILD_URL}">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>"</p>""",
      to: 'jp@matsusoft.com'
  )
}

def notifyFailed() {
  emailext (
      subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at "<a href="${env.BUILD_URL}">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>"</p>""",
      to: 'jp@matsusoft.com'
  )
}
