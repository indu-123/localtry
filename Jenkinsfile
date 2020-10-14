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
                agent{
                    docker{
                        image 'maven'
                        image 'docker-compose:latest'
                        args '--network localtryprojekt -v /var/run/docker.sock:/var/run/docker.sock'
                    }
                    environment {
                        HOME="."
                    }
                }
                steps {
                    sh 'docker-compose up -d'
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
                    sh 'mvn clean package'
                }
            }    
            
        }
}
