# example-kaniko

## Description

A Pipeline example of running Kaniko. It writes a Dockerfile inside the kaniko container and builds
the Dockerfile to an image and pushes it to a local private registry.

## Requirements

Kubernetes Secret named `regcred` contained within a volume which gets mounted to /kaniko/.docker .
Local private docker registry.

## Dependencies

[Kubernetes Plugin](https://github.com/jenkinsci/kubernetes-plugin)
