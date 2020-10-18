String maven = "maven:3.6.3-adoptopenjdk-14"
pipeline {
    agent any
        environment{
            NEXUS_HOST = 'nexus:8081'
            NEXUS_USERNAME = 'admin'
            NEXUS_PASSWORD = 'admin'
            SONARQUBE_HOST = 'sonarqube:9000'
        }
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
            stage("Build Pipeline Slaves via Docker-Compose") {
                agent any
                steps {
                    sh 'docker-compose up -d'
                }
            }
            stage("Maven Install") {
                agent {
                    docker {
                        image 'maven'               
                    }
                }
                steps {
                    sh 'mvn clean install'
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
                    sleep(10)
                    sh 'mvn clean package'
                    sh 'ls target'
                }
            } 
            stage("deploy to sonarqube") {
                agent {
                    docker {
                        image 'maven'
                        args '--network localtryprojekt'
                    }
                }
                steps {
                    sh 'mvn -s settings.xml clean verify sonar:sonar -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=1881e72c76288e59801498bc865c9d40efb1cd4d -Dsonar.projectName=localtry -Dsonar.projectVersion=1'
                }
            }
            stage("deploy to nexus") {
                agent {
                    docker {
                        image 'maven'
                        args '--network localtryprojekt'
                    }
                }
                steps {
                    withCredentials([usernamePassword(credentialsId: 'nexus-credentials', passwordVariable: 'NEXUS_PASSWORD', usernameVariable: 'NEXUS_USER')]) {                
                        sh 'mvn -s settings.xml deploy'
                    }
                }
            }

/*            stage("deploy to nexus using configFileProvider"){
                agent {
                    docker {
                        image 'maven'
                    }
                }
                steps {
                    configFileProvider(
                        [configFile(fileId: 'indusettings', variable: 'MAVEN_SETTINGS')]) {
                        }
                        sh 'mvn -s $MAVEN_SETTINGS clean deploy'
                        args '--network localtryprojekt -v /var/run/docker.sock:/var/run/docker.sock'
                }
            }*/
/*            stage("deploy War-file to tomcat") {
                steps {
                    ansiblePlaybook colorized: true, disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory', playbook: 'deploy.yml'
                }
            } */  
/*            stage("deploy War-file to tomcat") {
                agent {
                    docker {
                        image 'maven'
                    }
                }
                steps {
                    sh 'mvn -s settings.xml tomcat7:deploy'
                }
            }*/
        }  
}
