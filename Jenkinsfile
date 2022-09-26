pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    tools{
        maven 'mavenLocal'
    }
    stages {
         stage('Clone repository') { 
            steps { 
                script{
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github_credentials', url: 'https://github.com/enzopcastillo/devops-automation']]])
                    sh 'mvn clean install'
                }
            }
        }
        stage('Build') { 
            steps { 
                script{
                 app = docker.build("enzopcastillo/devops-test")
                }
            }
        }
        stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://registry.hub.docker.com/', 'dockerhub_credentials') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}