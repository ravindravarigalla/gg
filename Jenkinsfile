pipeline {

  environment {
    PROJECT = "	still-smithy-279711"
    APP_NAME = "sample"
    FE_SVC_NAME = "${APP_NAME}"
    CLUSTER = "cluster-1"
    CLUSTER_ZONE = "us-central1-c"
    IMAGE_TAG = "gcr.io/${PROJECT}/${APP_NAME}:latest"
    JENKINS_CRED = "${PROJECT}"
  }

  agent {
    kubernetes {
      
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:v1.0.0
    imagePullPolicy: Always
    args: ["--dockerfile=Dockerfile",
            "--context=https://github.com/ravindravarigalla/sample333.git",
            "--destination=ravindra777/dockertest:v1"]
    volumeMounts:
      - name: docker-config
        mountPath: /kaniko/.docker
    resources:
      limits:
        cpu: 1
        memory: 1Gi
  volumes:
  - name: docker-config
    projected:
      sources:
      - secret:
          name: regcred
          items:
            - key: .dockerconfigjson
              path: config.json
    
"""
}
  }
  stages {
    stage('Test') {
      steps {
        container('kaniko') {
          sh """
          /kaniko/executor --dockerfile `pwd`/Dockerfile --context `pwd` --destination=ravindra777/dockertest
          """
        }
      }
    }
  }
}
