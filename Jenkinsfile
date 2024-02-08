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
        nexusURL = '172.31.5.103:8081'
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
                    zip -r -q catalogue.zip ./* -x ".git" - x "*.zip"  
                    ls -ltr                          
                """                                         ///-q is used for hide the logs in console
                }                    
            }
         stage('publish artifact') {
            steps {
                nexusArtifactUploader(                            //publish artifacts here
                            nexusVersion: 'nexus3',
                            protocol: 'http',
                            nexusUrl: "${nexusURL}",
                            groupId: 'com.roboshop',
                            version: "${packageVersion}",         
                            repository: 'catalogue',
                            credentialsId: 'nexus-auth',
                            artifacts: [
                                [artifactId: 'catalogue',
                                classifier: '',
                                file: 'catalogue.zip',
                                type: 'zip']
                            ]
     )
            }

    }
    stage('deploy') {
            steps {
                script {
                    def params = [
                        string(name: 'version' , value: "${packageVersion}"),
                         string(name: 'environment' , value: "dev")
                    ]
                }
               build jod: "catalogue-deploy-1", wait: true, parameters: params
                
            }
    }

}
}



////after downloadin artifacts into nexus we gwet pom.xmlfile....the pom file contains group.id,artifat id ,version,type
//////till artifact creation that we called as CI