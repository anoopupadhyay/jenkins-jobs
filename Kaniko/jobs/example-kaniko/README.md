# example-kaniko

## Description

A Pipeline example of running Kaniko. It writes a Dockerfile inside the kaniko container and builds
the Dockerfile to an image and pushes it to a local private registry.

## Requirements

Kubernetes Secret named `regcred` contained within a volume which gets mounted to /kaniko/.docker .
Local private docker registry.

## Dependencies

[Kubernetes Plugin](https://github.com/jenkinsci/kubernetes-plugin)

## Jenkinsfile (or Pipeline)

```
podTemplate(yaml: """
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug-9912ccbf8d22bbafbf971124600fbb0b13b9cbd6
    imagePullPolicy: IfNotPresent
    command:
    - /busybox/cat
    tty: true
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /kaniko/.docker
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: regcred
          items:
            - key: .dockerconfigjson
              path: config.json
"""
  ) {

  node(POD_LABEL) {
    stage(&apos;Build with Kaniko&apos;) {
      container('kaniko') {
        writeFile file: "Dockerfile", text: """
            FROM jenkins/slave:3.29-2
            MAINTAINER CloudBees Support Team <dse-team@cloudbees.com>
            RUN mkdir /home/jenkins/.m2
        """
        sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --cache=true --destination=10.104.133.107:5003/dchang/scratch'
      }
    }
  }
}
```
