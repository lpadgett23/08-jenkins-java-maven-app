pipeline {
    agent any
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description:'')
        booleanParam(name: 'executeTests', defaultValue: true, description:'')
    }
    stages {
        stage("build") {
            steps {
                echo 'building the application...'
            }
        }
        stage("test") {
           when {
               expression {
                   params.executeTests 
               }
           }
            steps {
                echo 'testing the application...'
            }
        }
        stage("deploy") {   
            steps {
                echo 'deploying the application...'
                echo "deploying version ${params.VERSION}"
                // echo "deploying with ${SERVER_CREDENTIALS}"
                // sh "${SERVER_CREDENTIALS}"
                // better practice -- instead, use a wrapper approach thx to credentials plugin + credentials binding plugin:
                // withCredentials([
                //     usernamePassword(credentials: 'server-credentials-TEST', usernameVariable: USER, passwordVARIABLE: PWD)
                // ]) {
                //     sh "some script ${USER} ${PWD}"
                // }
            }
        }
    }
    // post { } block       // always, success, failure -- build status or build status changes can be conditions for the post block
}
