apiVersion: v1
kind: Template
labels:
  application: hellopythonapp
metadata:
  name: hellopythonapp
objects:
  -
    apiVersion: v1
    kind: BuildConfig
    metadata:
      labels:
        build: hellopythonapp
      name: hellopythonapp
    spec:
      runPolicy: Serial
      source: {}
      strategy:
        jenkinsPipelineStrategy:
          jenkinsfile: |-
            pipeline {
              node {
                stage('build & deploy') {
                  openshiftBuild bldCfg: 'hellopythonapp',
                    namespace: 'development',
                    showBuildLogs: 'true'
                  openshiftVerifyDeployment depCfg: 'hellopythonapp',
                    namespace: 'development'
                }
                stage('approval (test)') {
                  input message: 'Approve for testing?',
                    id: 'approval'
                }
                stage('deploy to test') {
                  openshiftTag srcStream: 'hellopythonapp',
                    namespace: 'development',
                    srcTag: 'latest',
                    destinationNamespace: 'testing',
                    destStream: 'hellopythonapp',
                    destTag: 'test'
                  openshiftVerifyDeployment depCfg: 'hellopythonapp',
                    namespace: 'testing'
                }
                stage('approval (production)') {
                  input message: 'Approve for production?',
                    id: 'approval'
                }
                stage('deploy to production') {
                  openshiftTag srcStream: 'hellopythonapp',
                    namespace: 'development',
                    srcTag: 'latest',
                    destinationNamespace: 'production',
                    destStream: 'hellopythonapp',
                    destTag: 'prod'
                  openshiftVerifyDeployment depCfg: 'hellopythonapp',
                    namespace: 'production'
                }   
         }
         type: JenkinsPipeline
