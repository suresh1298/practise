pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage ("scm checkout") {
            parallel {
               stage ("git") {
                   steps {
                       node ("jenkins_slave") {
                           git credentialsId: '94890d65-8c98-4dfa-8dcd-1529d5e94fed', url: 'https://github.com/suresh1298/practise'
                       }
                   }
               }
               stage ("null") {
                   steps {
                       node ("jenkins_slave") {
                           sh "echo 'suresh'"
                       }
                   }
               }
               stage ("print") {
                   steps {
                       node ("jenkins_slave") {
                           sh "echo 'print'"
                       }
                   }
               }
            }
        }
        stage ("scan") {
            environment {
                scannerHome = tool 'sonar_slave'
            }
            steps {
                node ("jenkins_slave") {
                    withSonarQubeEnv('sonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage ("quality") {
            steps {
                node ("jenkins_slave") {
                    timeout(time: 1, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true   
                    }
                }
           }
        }
        stage ("buld") {
            steps {
                node ("jenkins_slave") {
                    sh 'mvn clean install'
                }
            }
        }
        stage ('nexus') {
            steps {
                node ("jenkins_slave") {
                    nexusPublisher nexusInstanceId: 'nexus', nexusRepositoryId: 'sample_release', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '.war', filePath: 'target/simple-web-app.war']], mavenCoordinate: [artifactId: 'simple-web-app', groupId: 'happy', packaging: 'war', version: '1.4']]]
                }
            }
        }
        stage ("deploy") {
            steps {
                node ("jenkins_slave") {
                    sh "sudo cp target/*.war /opt/tomcat/webapps"
                }
            }
        }
    }
}
