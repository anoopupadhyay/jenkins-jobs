# maven-inplace-pipeline

## Description

Pipeline which defines a pod template in-place (with no dependencies on shared clouds or defaults)

This pipeline represents the minimum required in order to launch a pod template (minus the
hostPath volume, i.e. docker.sock)

## Requirements

## Dependencies

## Jenkinsfile (or Pipeline)

```
podTemplate(label: 'inplace', containers: [
  containerTemplate(name: 'jnlp', image: 'maven:3.6.2-jdk-8-slim', command: 'sh', args: '/var/jenkins_config/jenkins-agent'),
  ], volumes: [
        configMapVolume(mountPath: '/var/jenkins_config', configMapName: 'jenkins-agent'),
        hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
  ],) {

  node('inplace') {
    stage('Build a Maven project') {
      container('jnlp') {
          sh 'ls /var/run/docker.sock'
          sh 'mvn --version'
      }
    }
  }
}
```
