# github-dns-pipeline

## Description

Pipeline job which tests dns

The master initially performs a checkout of the repo (for the Jenkinsfile).
However some versions of Jenkins (on Alpine base images) has an issue with
DNS and results in failure to checkout from github with

```
Running in Durability level: MAX_SURVIVABILITY
[Pipeline] Start of Pipeline
[Pipeline] podTemplate
[Pipeline] {
[Pipeline] node
Agent github-dns-65zbj-z6xx7 is provisioned from template Kubernetes Pod Template
Agent specification [Kubernetes Pod Template] (github-dns): 
* [jnlp] jenkins/jnlp-slave:alpine
* [maven] maven:3.6.2-jdk-8-slim

Running on github-dns-65zbj-z6xx7 in /home/jenkins/workspace/github-dns-pipeline
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Build a Maven project)
[Pipeline] git
No credentials specified
Cloning the remote Git repository
Cloning repository https://github.com/cbdchang/library-resources.git
ERROR: Error cloning remote repo 'origin'
hudson.plugins.git.GitException: Command "git fetch --tags --force --progress https://github.com/cbdchang/library-resources.git +refs/heads/*:refs/remotes/origin/*" returned status code 128:
stdout: 
stderr: fatal: unable to access 'https://github.com/cbdchang/library-resources.git/': Could not resolve host: github.com
```

The DNS issue is intermittent as I experienced this error yesterday but today DNS is fine

## Requirements

## Dependencies

## Jenkinsfile (or Pipeline)

```
podTemplate(label: 'github-dns', containers: [
  containerTemplate(name: 'maven', image: 'maven:3.6.2-jdk-8-slim', ttyEnabled: true, command: 'cat')
  ]) {

  node('github-dns') {
    stage('Build a Maven project') {
      git 'https://github.com/cbdchang/library-resources.git'
      container('maven') {
          sh 'curl -v https://github.com'
          sh 'mvn --version'
      }
    }
  }
}
```
