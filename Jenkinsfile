// Declarative //
pipeline {
    agent {
        node {
            label 'AGENT-1'
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
        stage('install dependencies') {
            steps {
                sh """
                    npm install
                """
                }
            }
            stage('build') {                      ////here we have to zip the files thst is schema,package.jsos
            steps {                                 /// server.js//zip the file and floders..catalogue.zip is the 
                                                      ///zip file and i exclude .git and .zip file within that files
                sh """
                    ls -al 
                    zip -r catalogue.zip ./* -x ".git" - x "*.zip"  
                    ls -ltr                          
                """
                }
            }
        
    }
}