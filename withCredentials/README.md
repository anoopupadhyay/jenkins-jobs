# withCredentials

## Description

Uses the withCredentials keyword to make a Jenkins credential available in
a pipeline step.

## Requirements

## Dependencies

## Jenkinsfile (Pipeline)

```
pipeline {
  agent {
    kubernetes {
        label 'centos6-py36'
        slaveConnectTimeout "200"
        yaml """
metadata:
  name: centos6-py36
  labels:
    jenkins: "slave"
spec:
  containers:
  - name: "centos6-py36"
    image: "centos6-python3.6.9:latest"
    imagePullPolicy: IfNotPresent
    command: ["cat"]
    tty: true
"""
        }
    }
    stages {
        stage('Build Package') {
            steps {
                container('centos6-py36') {
                    // TODO: Probably also ought to throw some linting in here.
                    sh '/usr/local/bin/python3.6 -V'
                }
            }
        }
        stage('Publish Package') {
            steps {
                container('centos6-py36') {
                    // dont use 'PWD' as passwordVariable
                    withCredentials([usernamePassword(credentialsId: 'dchang', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh 'printenv'
                    }                    
                }
            }
        }
    }
}
```
