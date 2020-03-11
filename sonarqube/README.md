# sonarqube

## Description

Pipeline job which installs Sonar and Maven and runs a build and scan.

## Requirements

SonarQube Server installed, e.g. http://localhost:9000

## Dependencies

[SonarQube Scanner Plugin](https://github.com/SonarSource/sonar-scanner-jenkins)

## Jenkinsfile (or Pipeline)

```
pipeline {
    agent any
    tools {
        maven 'maven3';
    }
    environment {
        scannerHome = tool 'sonarqube4';
    }
    stages {
        stage ('Checkout') {
            steps {
                checkout([$class: 'GitSCM', 
                branches: [[name: '*/master']], 
                doGenerateSubmoduleConfigurations: false, 
                extensions: [[$class: 'CleanCheckout']], 
                submoduleCfg: [], 
                userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/cbdchang/simple-java-maven-app.git']]
            ])  
            }
        }
        stage ('Initialize') {
            steps {
                sh '''
                    echo 'PATH = ${PATH}'
                    echo 'M2_HOME = ${M2_HOME}'
                '''
            }
        }
        stage ('Build and Scan') {
            steps {
                withSonarQubeEnv(installationName: 'sonarqube') { // You can override the credential to be used
                    sh 'mvn -DskipTests -B clean package sonar:sonar'
                }
            }
        }
    }
}
```
