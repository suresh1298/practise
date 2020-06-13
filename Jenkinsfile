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
                                     nexusPublisher nexusInstanceId: 'nexus', nexusRepositoryId: 'sample_snapshot', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/practise.war']], mavenCoordinate: [artifactId: 'practise', groupId: 'whatsapp', packaging: 'war', version: '1.0']]]
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
