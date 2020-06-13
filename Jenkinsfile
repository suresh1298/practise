pipeline {
    agent any
    stages {
        stage ("SCM_CHECKOUT") {
            steps {
                node ("${NODE}") {
                    script {
                        if (env.BRANCH_NAME == 'master') {
                            git credentialsId: '7778fd25-578d-48df-b454-17fe5ca8baa0', url: 'https://github.com/suresh1298/practise'
                        } else {
                            git branch: 'develope', credentialsId: '7778fd25-578d-48df-b454-17fe5ca8baa0', url: 'https://github.com/suresh1298/practise.git'
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
                node ("${node}") {
                    withSonarQubeEnv('sonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage ("quality") {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage ("maven") {
            parallel {
                stage ("buld in master") {
                    steps {
                        sh 'mvn clean install'
                    }
                }
                stage ("buld in slave") {
                    steps {
                        node ("${NODE}") {
                            sh 'mvn clean install'
                        }
                    }
                }
            }
        }
        stage ("nexus") {
            parallel {
                stage ("deploy in master") {
                    steps {
                        node ("${NODE}") {
                            script {
                                 if (env.NEXUS == 'VERSION') {
                                     nexusPublisher nexusInstanceId: 'nexus', nexusRepositoryId: 'sample_release', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/practise.war']], mavenCoordinate: [artifactId: 'practise', groupId: 'whatsapp', packaging: 'war', version: '1.0']]]
                                 } else {
                                     nexusArtifactUploader artifacts: [[artifactId: 'practise', classifier: '', file: 'target/practise.war', type: 'war']], credentialsId: '5b2035fb-2986-44bb-8ff1-8c433fa1d996', groupId: 'whatsapp', nexusUrl: '3.235.173.182', nexusVersion: 'nexus3', protocol: 'http', repository: 'http://3.235.173.182:8081/repository/sample_snapshot/', version: '1.0'
                                 }
                            }
                        }
                    }
                }
                stage ("sucess") {
                    steps {
                        sh "echo 'sucess from nexus'"
                    }
                }
            }
        }
        stage ("depeloy") {
            parallel {
                stage ("deploy in jenkins") {
                    steps {
                        sh "cp target/*.war /opt/tomcat/webapps"
                    }
                }
                stage ("deploy in slave") {
                    steps {
                        node ("${NODE}") {
                            sh "sudo cp target/*.war /opt/tomcat/webapps"
                        }
                    }
                }
            }
        }
    }
}
