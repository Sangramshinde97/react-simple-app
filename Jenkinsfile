pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
               sh "npm install"
               sh "npm run build"
            )
                }
            }
         stage('Unit Test maven'){
            steps{
               script{
                 mvnTest()
               }
            }
        }
        }
    }
