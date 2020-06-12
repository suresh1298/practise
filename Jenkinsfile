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
                       node ("master") {
                           git credentialsId: '7778fd25-578d-48df-b454-17fe5ca8baa0', url: 'https://github.com/suresh1298/practise'
                       }
                   }
               }
               stage ("null") {
                   steps {
                       node ("master") {
                           sh "echo ${a}"
                       }
                   }
               }
               stage ("print") {
                   steps {
                       node ("master") {
                           sh "echo 'print'"
                       }
                   }
               }
            }
        }
        stage ("scan") {
            environment {
                scannerHome = tool 'sonar'
            }
            steps {
                node ("master") {
                    withSonarQubeEnv('sonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage ("quality") {
            steps {
                node ("master") {
                    timeout(time: 1, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true   
                    }
                }
           }
        }
        stage ("buld") {
            steps {
                node ("master") {
                    sh 'mvn clean install'
                }
            }
        }
        stage ('nexus') {
            steps {
                node ("master") {
                    nexusPublisher nexusInstanceId: 'nexus', nexusRepositoryId: 'sample_release', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: 'war', filePath: 'target/practise.war']], mavenCoordinate: [artifactId: 'practise', groupId: 'whatsapp', packaging: 'war', version: '1.5']]], tagName: '1.6'
                }
            }
        }
        stage ("deploy") {
            steps {
                node ("master") {
                    sh "sudo cp target/*.war /opt/tomcat/webapps"
                }
            }
        }
    }
}
