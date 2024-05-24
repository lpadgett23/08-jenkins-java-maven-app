pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }
    stages {
        stage('Increment version') {
            steps {
                script {
                    echo "Incrementing the app version... (i.e. creating new image version, updating IMAGE_NAME env var for global use)"
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>' // groovy lang willl save what it finds in an array
                    def version = matcher[0][1]  // this is simply groovy lang, i.e. how to access down to the text itself that's between the two version tags
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER" // $BUILD_NUMBER is an env var that jenkins makes available to us at each pipeline, ie the #__ one you see in your build history
                                        // appending build_number as a suffix to the version num is one of the common approaches to tagging images (but there are multiple ways/standards for how this can be done)
                }
            }
        }
        stage('Build app') {
            steps {
                script {
                    echo "Building the application..."
                    sh 'mvn clean package' // deletes the tar already in the target folder
                }
            }
        }
        stage('Build image') {
            steps {
                script {
                    echo "Building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "docker build -t lepcloud23/demo-app:${IMAGE_NAME} ."
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh "docker push lepcloud23/demo-app:${IMAGE_NAME}"
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
        stage('Commit version update') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'

                        sh 'git status'
                        sh 'git branch'
                        sh 'git config --list'
                        sh "git remote set-url origin https://${USER}:${PASS}@github.com/lpadgett23/08-jenkins-java-maven-app.git"
                        sh 'git add .'
                        sh 'git commit -m "ci: version bump"'
                        sh 'git push origin HEAD:jenkins-jobs-2nd'  
                    }
                }
            }
        }
    }
}
