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
        nexusURL = '172.31.12.44:8081'
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
                    sudo dnf install zip -y
                    zip -q -r catalogue.zip ./* -x ".git" -x "*.zip"
                    ls -ltr
                """
            }
        }
        stage('Uploading the Artifacts to Nexus') {
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
                        [artifactId: 'catalogue',
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]
                )
            }
        }
        stage('Giving the Package Version & Environment to CATALOGUE-CD') {
            steps {
                build job: 'Catalogue-CD', 
                parameters: [
                    string(name: 'ENVIRONMENT', value: 'dev'),
                    string(name: 'VERSION', value: "${packageVersion}")
                ]
            }
        }
    }
    post {
        always {
            echo 'PIPELINE EXECUTION IS COMPLETED'
        }
        failure {
            echo 'The pipeline is FAILED'
        }
        success {
            echo 'The pipeline is SUCESS'
        }
    }
}