#!/usr/bin/env groovy

pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }
    stages {
        stage('Build app') {
            steps {
                script {
                    echo "Building the application..."
                    sh 'mvn package'
                }
            }
        }
        stage('Build image') {
            steps {
                script {
                    echo "Building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh 'docker build -t lepcloud23/demo-app:jma-3.3.5 .'
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh 'docker push lepcloud23/demo-app:jma-3.3.5'
                    }
                    echo "Pushed the docker image to docker hub demo-app repo"
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploying docker image..."  
                }
            }
        }
    }
}
