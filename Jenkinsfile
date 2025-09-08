pipeline {
    agent { label 'AGENT-1' }

    environment {
        REGION = "us-east-1"
        ACC_ID = "253490767945"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }
    options {
        disableConcurrentBuilds()
        timeout(time: 30, unit: 'MINUTES')
    }

    parameters {
        booleanParam(name: 'deploy', defaultValue: false, description: 'Toggle this value')
    }

    stages {
        stage('Build') {
            steps {
                script {

                    sh """
                        echo 'Hello Build'
                        sleep 10
                        env
                        echo "I am learning ${Course}. Wish me  best"
                        echo "Hello ${params.PERSON}"
                    """
                }
               

            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
              input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }

            steps {
                
                script {

                    echo "Hello, ${PERSON}, nice to meet you."
                    echo 'Deploying....'
                }
            }
        }
    }

    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        success { 
            echo 'Hello Success'
        }
        failure { 
            echo 'Hello Failure'
        }
    }
}