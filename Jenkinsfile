#!/usr/bin/groovy

def registry = 'forgejo.sakul-flee.de'
def owner = 'HelmCharts'

def target = "https://${registry}/api/packages/${owner}/helm/api/charts"

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

    stage('Package') {
      steps {
        container('helm') {
          sh "helm package ."
        }
      }
    }

    stage('Release') {
      steps {
        container('helm') {
          script {
            // Extract version from Chart.yaml to identify the file name
            def version = sh(script: "grep '^version:' Chart.yaml | awk '{print \$2}'", returnStdout: true).trim()
            def name = sh(script: "grep '^name:' Chart.yaml | awk '{print \$2}'", returnStdout: true).trim()
            def pkgFile = "${name}-${version}.tgz"
            
            def token = sh(script: "cat /var/run/secrets/additional/secret-jenkins-forgejo-token/token", returnStdout: true).trim()

            sh """
              curl --user "jenkins:${token}" \
                -X POST \
                --upload-file ./${pkgFile} \
                ${target}
            """
          }
        }
      }
    }
  }
}
