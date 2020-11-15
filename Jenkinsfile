pipeline {
    agent any
    stages{
        stage ('Build Backend'){
            steps {
                sh 'mvn clean package -DskipTest=true'
            }
        }
        stage ('Unit Tests'){
            steps {
                sh 'mvn test'
            }
        }
        stage ('Deploy Backend'){
            steps {
                deploy adapters: [tomcat8(credentialsId: '4c973e5c-8adf-4cfb-a621-191b000e2233', path: '', url: 'http://172.31.93.3:8080/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war' 
            }
        }
        stage ('Deploy Frontend'){
            steps {
                dir('frontend'){
                    git 'https://github.com/gabrielhmc/tasks-frontend.git'
                    sh 'mvn clean package -DskipTest=true'
                    deploy adapters: [tomcat8(credentialsId: '4c973e5c-8adf-4cfb-a621-191b000e2233', path: '', url: 'http://172.31.93.3:8080/')], contextPath: 'tasks', war: 'target/tasks.war'
                }                
            }
        }
        stage ('Deploy Prod'){
            steps {
                sh 'docker-compose build'
                sh 'docker-compose up -d'
            }
        }
    }
}


