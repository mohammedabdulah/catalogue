pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        packageVersion = ''
    }
    options {
        timeout (time : 1 , units: 'HOURS')
        disableConcurrentBuilds()
    }
    stages {
        stage ('GET THE VERSION') {
            steps {
                script {
                    def packagejson = readjsonfile : 'package.json'
                    packageVersion= pacagejson.version
                    echo "application version : $packageVersion"
                }
            }
        }
    }
}