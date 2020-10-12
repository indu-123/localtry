String maven = "maven:3.6.3-adoptopenjdk-14"
pipeline {
    agent any
        stages {    
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
            stage('Build Pipeline Slaves via Docker-Compose') {
                agent {
                    docker {
                        image 'docker/compose:latest'
                        args '--network localtryprojekt -v /var/run/docker.sock:/var/run/docker.sock'
                    }
                }
                steps {
                    sh 'docker-compose build && docker-compose up -d'
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
 









pipeline {
    agent none
    environment {
        NEXUS_HOST = 'nexus:8081'
    }
    stages {
        stage('unit tests') {
            steps {
                echo 'unit tests'
            }
         }
        stage('nexus upload') {
            steps {
            	echo 'nexus upload'
            }
        }
        stage('artifact package') {
            steps {
                echo 'artifact package'
            }
        }
        stage('container runs') {
            steps {
            	echo 'container runs'
            }
        }
        stage('integration tests') {
            steps {
            	echo 'integration tests'
            }
        }
        stage('container stops') {
            steps {
            	echo 'container stops'
            }
        }
    }
}
