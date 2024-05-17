pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        packageVersion = ''
        nexusURL = ''
    }
    options {
        timeout (time : 1 , unit: 'HOURS')
        disableConcurrentBuilds()
    }
    stages {
        stage('Get the version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    packageVersion = packageJson.version
                    echo "application version: $packageVersion"
                }
            }
        }
        stage ('Install Dependencies') {
            steps {
                sh """
                npm install
                """
            }
        }
        stage ('Build') {
            steps {
                sh """ 
                ls -la
                zip -q -r catalogue.zip ./* -x ".git" -x "*.zip"
                ls -ltr
                """
            }
        }
        stage ('publish artifact') {
            steps {
                nexusArtifactUploader(
                nexusVersion: 'nexus3',
                protocol: 'http',
                nexusUrl: "${nexusURL}",
                groupId: 'com.roboshop',
                version: "${packageVersion}",
                repository: 'catalogue',
                credentialsId: 'nexus-auth',
                artifacts: [
                    [artifactId: catalogue,
                    classifier: '',
                    file: 'catalogue.zip',
                    type: 'zip']
                ]
            )
            }
        }
        stage ('Deploy') {
            steps {
                script {
                    def params= [
                         string (name: 'version' , value: "$packageVersion"),
                         string (name: 'environment' , value: "dev")
                    ]
                    build job : "catalogue-deploy" , wait: true , parameters: params
                }
            }
        }
        }
        post {
            always {
                deleteDir()
            }
        }
    
}