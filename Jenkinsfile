#!/usr/bin/groovy

def registry = 'forgejo.sakul-flee.de'
def namespace = 'container'
def chartName = 'hytale'
def chartTarget = "oci://${registry}/${namespace}"

pipeline {
  agent {
    kubernetes {
      yaml """
      apiVersion: v1
      kind: Pod
      metadata:
        name: helm-build-pod
        namespace: jenkins
      spec:
        containers:
        - name: helm
          image: alpine/helm:3.12.0
          command: ['cat']
          tty: true
          volumeMounts:
          - name: forgejo-token
            mountPath: /var/run/secrets/additional/secret-jenkins-forgejo-token
            readOnly: true
        volumes:
        - name: forgejo-token
          secret:
            secretName: secret-jenkins-forgejo
      """
    }
  }

  stages {
    stage('Lint') {
      steps {
        container('helm') {
          sh "helm lint ."
        }
      }
    }

    stage('Login') {
      steps {
        container('helm') {
          sh "cat /var/run/secrets/additional/secret-jenkins-forgejo-token/token | helm registry login ${registry} --username jenkins --password-stdin"
        }
      }
    }

    stage('Release Chart') {
      steps {
        container('helm') {
          script {
            // Extract version from Chart.yaml to identify the file name
            def chartVersion = sh(script: "grep '^version:' Chart.yaml | awk '{print \$2}'", returnStdout: true).trim()
            def pkgFile = "${chartName}-${chartVersion}.tgz"
            
            // Package and Push
            sh "helm package ."
            sh "helm push ${pkgFile} ${chartTarget}"
            
            echo "Successfully released ${chartName} version ${chartVersion} to ${chartTarget}"
          }
        }
      }
    }
  }
}
