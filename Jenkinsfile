pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    } 
    options {
        disableConcurrentBuilds()
        ansiColor('xterm')
        timeout(time: 1, unit: 'HOURS')
    }
    environment {
        packageVersion = ''
    }
    stages {
        stage('Getting the Package Version') {
            steps {
                script {
                    def packageJSON = readJSON file: 'package.json'
                    packageVersion = packageJSON.version
                    echo "App Version is $packageVersion"
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline execution is COMPLETED'
        }
        failure {
            echo 'The pipeline is FAILED'
        }
        success {
            echo 'The pipeline is SUCESS'
        }
    }
}