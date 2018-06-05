#!groovy​

// The 'xchem' Fragalysis Stack Jenkinsfile.

pipeline {

  agent { label 'buildah-slave' }

  environment {
    // Registry details
    USER = 'jenkins'
    REGISTRY = 'docker-registry.default:5000'
    STREAM_IMAGE = "${REGISTRY}/fragalysis-cicd/fragalysis-stack:latest"
  }

  stages {

    stage('Inspect') {
      steps {
          echo "Inspecting..."
      }
    }

    stage('Build Image') {
      steps {
        echo "Building fragalysis-stack..."
        sh "buildah bud --format docker -f Dockerfile -t ${STREAM_IMAGE}"
      }
    }

    stage('Push Image') {
      steps {
        script {
          TOKEN = sh(script: 'oc whoami -t', returnStdout: true).trim()
        }
        sh "podman login --tls-verify=false --username ${env.USER} --password ${TOKEN} ${env.REGISTRY}"
        sh "buildah push --tls-verify=false ${env.STREAM_IMAGE} docker://${env.STREAM_IMAGE}"
        sh "podman logout ${env.REGISTRY}"
      }
    }

  }

}
