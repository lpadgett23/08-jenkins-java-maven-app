#!/user/bin/env groovy

@Library('jenkins-shared-library')  // you'll need to add an underscore at end of this decorator()_ like that. (IF you don't have a variable def right below it) (we have a var def on next line though so no need for _ in this case)
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
        stage('Build image') {
            steps {
                script {
                    buildImage "lepcloud23/demo-app:jma-3.1"
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