String maven = "maven:3.6.3-adoptopenjdk-14"
pipeline {
    agent any
        stages {
            stage('Build Pipeline Slaves via Docker-Compose') {
                steps {
                    sh 'docker-compose up -d'
                }
            }
            stage("Maven Compile") {
                agent {
                    docker {
                        image 'maven:3.6.3-adoptopenjdk-14'
                    }
                }
                steps {
                    sh 'mvn compile' 
                }
            }
            stage("Build & Unit Testing") {
                agent {
                    docker {
                        image 'maven:3.6.3-adoptopenjdk-14'
                    }
                }
                steps {
                    sh 'mvn test' 
                }
            }
            stage("WAR-File erstellen") {
                agent {
                    docker {
                        image 'maven'
                        args '--network localtryprojekt'
                    }
                }
                steps {
                    sleep(20)
                    sh 'mvn clean package'
                }
            }    
            
        }
}