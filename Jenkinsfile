def appVersion = ""

pipeline {
    agent { label 'AGENT-1' }

    environment {
        REGION   = "us-east-1"
        ACC_ID   = "253490767945"
        PROJECT  = "roboshop"
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
        stage('Read package.json') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "Package version: ${appVersion}"
                }
            }
        }

        stage('Install dependencies') {
            steps {
                sh "npm install"
            }
        }

        stage('Unit testing') {
            steps {
                sh "echo 'unit tests'"
            }
        }

        stage('Docker build') {
            steps {
                withAWS(credentials: 'aws-creds', region: "${REGION}") {
                    sh """
                        aws ecr get-login-password --region ${REGION} | \
                        docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.${REGION}.amazonaws.com

                        docker build -t ${ACC_ID}.dkr.ecr.${REGION}.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                        docker push ${ACC_ID}.dkr.ecr.${REGION}.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                    """
                }
            }
        }
    } // ✅ closes stages

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
    } // closes post
} // closes pipeline
