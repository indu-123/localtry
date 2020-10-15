String maven = "maven:3.6.3-adoptopenjdk-14"
pipeline {
    agent any
        stages {
            stage("Maven Compile") {
                agent {
                    docker {
                        image 'maven'
                    }
                }
                steps {
                    sh 'mvn compile' 
                }
            }
            stage("Build & Unit Testing") {
                agent {
                    docker {
                        image 'maven'
                    }
                }
                steps {
                    sh 'mvn test' 
                }
            }
            stage('Build Pipeline Slaves via Docker-Compose') {
                agent any
                steps {
                    sh 'docker-compose up -d'
                }
            }
            stage("WAR-File erstelle") {
                agent {
                    docker {
                        image 'maven'
                        args '--network localtryprojekt'
                    }
                }
                steps {
                    sh 'mvn clean package'
                }
            }  
            stage("deploy to nexus") {
                steps {
                    withCredentials([usernamePassword(credentialsId: 'nexus-credentials', passwordVariable: 'nexus_pass', usernameVariable: 'nexus_user')]) {                }
                        sh 'mvn deploy -s settings.xml'
                    }
                }
            }
            stage("deploy War-file to tomcat") {
                steps {
                    ansiblePlaybook colorized: true, disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory', playbook: 'deploy.yml'
                }
            }        
        }
}
