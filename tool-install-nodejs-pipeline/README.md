# tool-install-nodejs

## Description

Pipeline job which installs Maven and NodeJS

## Requirements

## Dependencies

[NodeJS Plugin](https://plugins.jenkins.io/nodejs/)

## Jenkinsfile (or Pipeline)

```
@Library('library-resources') _

pipeline {
  agent {
      kubernetes {
          label 'tool-install'
          yaml libraryResource('pod_templates/build-pod.yaml')
      }
  }
  tools {
      maven 'maven3'
  }
  stages {
    stage('Initialize') {
        steps {
            container('jnlp') {
                sh 'mvn --version'
            }
            container('nodejs') {
                nodejs(nodeJSInstallationName: 'nodejs13', configId: '49b932b7-952a-4f26-a084-d6a39b0c497c') {
                    sh 'printenv'
                    sh 'npm config ls'
                }
            }
        }
    }
  }
}
```
