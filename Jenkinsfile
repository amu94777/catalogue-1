// Declarative //
pipeline {
    agent {
        node {
            label = 'AGENT-1'
        }
    }
     options {
                timeout(time: 1, unit: 'HOURS')
                disableConcurrentBuilds() 
            }
     environment { 
        packageVersion = ''
    }
    stages {
        stage('get the version') {
            steps {
                script {
                      def packageJson = readJSON file: 'package.json'
                      packageVersion = packageJson.version
                      echo "the application version is :$packageVersion"
                }
            }
        }

    }
}