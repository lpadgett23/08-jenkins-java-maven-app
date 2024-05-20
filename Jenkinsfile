pipeline {
    agent any
    stages {
        stage('Test') {  // tests should be executed for all branches, no conditional logic needed cf other stages
            steps {
                script {
                    echo "Testing the application..."  
                    echo "Executing pipeline for branch ${BRANCH_NAME}"
                }
            }
        }
        stage('Build') {
            when {
                expression {
                    BRANCH_NAME == "main"
                }
            }
            steps {
                script {
                    echo "Building teh application..."
                    
                }
            }
        }
        stage('Deploy') {
            when {
                expression {
                    BRANCH_NAME == "main"
                }
            }
            steps {
                script {
                    echo "Deploying the application..."  
                }
            }
        }
    }
}
