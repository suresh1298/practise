pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage ("git scm") {
            steps {
                git 'https://github.com/suresh1298/practise'
            }
        }
        stage ("sonar") {
            environment {
                scannerHome = tool 'sonarscanner'
            }
            steps {
                withSonarQubeEnv('sonar') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage ("quality check") {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage ('maven build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage ('nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'whatsapp', classifier: '', file: '/target/practise.war', type: 'war']], credentialsId: '5a0af56a-1fa8-49a1-8d5f-bc059281da43', groupId: 'practise', nexusUrl: '52.66.198.195:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'suresh_release', version: '1.2.0'
            }
        }
        stage ('tomcat') {
            steps {
                sh 'sudo cp target/*.war /opt/tomcat/webapps/'
            }
        }
    }
    post {
        success {
            emailext (
                to: 'reddysuresh1298@gmail.com',
                subject: "JOB: ${env.JOB_NAME} - SUCCESS",
                body: "JOB SUCCESS - \"${env.JOB_NAME}\" Build No: ${env.BUILD_NUMBER}\n\nClick on the below link to view the logs:\n ${env.BUILD_URL}\n"
            )
        }
        failure {
            emailext (
                to: 'reddysuresh1298@gmail.com',
                subject: "JOB: ${env.JOB_NAME} - FAILURE",
                body: "JOB FAILURE - \"${env.JOB_NAME}\" Build No: ${env.BUILD_NUMBER}\n\nClick on the below link to view the logs:\n ${env.BUILD_URL}\n"
            )
        }
    }
}
