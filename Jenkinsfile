#!/user/bin/env groovy

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
                    echo "Building the docker image for branch ${BRANCH_NAME}..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', userVariable: 'USER')]) {
                        sh 'docker build -t lepcloud23/demo-app:jma-3.3.4 .'
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh 'docker push lepcloud23/demo-app:jma-3.3.4'
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
