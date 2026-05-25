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
        stage('Installing NodeJS') {
            steps {
                sh """
                   sudo  dnf module disable nodejs -y
                    sudo dnf module enable nodejs:20 -y
                    sudo dnf install nodejs -y
                """
            }
        }
        stage('Installing Dependencies') {
            steps {
                sh """
                    npm install
                """
            }
        }
        stage('Building the Artifacts') {
            steps {
                sh """
                    ls -la
                    zip -r -q catalogue.zip ./* -x ".git" -x "*.zip"
                    ls -ltr
                """
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