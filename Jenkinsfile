#!/user/bin/env groovy

library identifier: "jenkins-shared-library@main", retriever: modernSCM(
    [$class: 'GitSCMSource',
    remote: 'https://github.com/lpadgett23/jenkins-shared-library.git',
    credentialsId: 'github-credentials']) 

def gv

pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }
   stages {
        stage('init') {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage('Build jar') {
            steps {
                script {
                    buildJar()
                }
            }
        }
        stage('Build and push image') {
            steps {
                script {
                    buildImage "lepcloud23/demo-app:jma-3.3"
                    dockerLogin()
                    dockerPush "lepcloud23/demo-app:jma-3.3"
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }
}
