# maven-with-libraryresource

## Description

Pipeline which defines a pod template in-place (similar to maven-inplace-pipeline) but
using a library resource to define the pod template

## Requirements

## Dependencies

## Jenkinsfile (or Pipeline)

```
@Library('library-resources') _

podTemplate(name: 'inplace', label: 'inplace', yaml: libraryResource('pod_templates/build-pod.yaml')) {
    node('inplace') {
        container('jnlp') {
            sh 'printenv'
        }
    }
}
```

## Library Resource

### pod_templates/build-pod.yaml
```
# ==============================================================================
# Jenkins slaves/workers/nodes/agents pod definitions
# ==============================================================================
apiVersion: v1
kind: Pod
spec:
  containers:
  # ----------------------------------------------------------------------------
  # JNLP Node
  # ----------------------------------------------------------------------------
  - name: 'jnlp'
    securityContext:
      runAsUser: 1000
    image: "maven:3.6.2-jdk-8-slim"
    tty: true
    command:
    - sh
    args:
    - /var/jenkins_config/jenkins-agent
    volumeMounts:
    - mountPath: /var/jenkins_config
      name: jenkins-agent
  volumes:
  - configMap:
    name: jenkins-agent
    path: /var/jenkins_config
```
