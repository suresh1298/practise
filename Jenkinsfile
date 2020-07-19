pipeline {
    agent any 
    stages {
        stage ("git") {
            steps {
                git 'https://github.com/suresh1298/practise.git'
            }
        }
        stage ("sonar analasys") {
            environment {
                scannerHome = tool 'scanner'
            }
            steps {
                script {
                    withSonarQubeEnv('sonar') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage ("quality gate check") { 
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage ("mvn") {
            steps {
                sh "sudo cp /opt/docker/java/java.sh ."
                sh "sudo cp /opt/docker//maven/maven.sh ."
                sh "sudo cp /opt/docker/tomcat/manager.xml ."
                sh "sudo cp /opt/docker/tomcat/tomcat-users.xml ."
            }
        }
        stage ("imagee build") {
            steps {
                script {
                    docker.build ("tomcat:latest", "--build-arg CACHE=9 .")
                }
            }
        }
        stage ("ecr") {
            steps {
                sh "sudo aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 340043406172.dkr.ecr.us-east-1.amazonaws.com"
                sh "sudo docker tag tomcat:latest 340043406172.dkr.ecr.us-east-1.amazonaws.com/practise:latest"
                sh "sudo docker push 340043406172.dkr.ecr.us-east-1.amazonaws.com/practise:latest"
            }
        }
        stage ("deployment") {
            steps {
                sh "aws ecs update-service --cluster tomcat --service  practise --force-new-deployment --region us-east-1 "
            }
        }
    }
    post {
        success {
            emailext (
                to: 'naramreddydileepreddy@gmail.com',
                subject: "JOB: ${env.JOB_NAME} - SUCCESS",
                body: "JOB SUCCESS - \"${env.JOB_NAME}\" Build No: ${env.BUILD_NUMBER}\n\nClick on the below link to view the logs:\n ${env.BUILD_URL}\n"
            )
        }
        failure {
            emailext (
                to: 'reddysuresh938@gmail.com',
                subject: "JOB: ${env.JOB_NAME} - FAILURE",
                body: "JOB FAILURE - \"${env.JOB_NAME}\" Build No: ${env.BUILD_NUMBER}\n\nClick on the below link to view the logs:\n ${env.BUILD_URL}\n"
            )
        }
    }
}
