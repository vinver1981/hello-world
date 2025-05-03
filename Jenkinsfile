pipeline {
    agent any

    environment {
        // Define variables
        MVN_CMD = 'mvn'
        WAR_NAME = '	webapp.war'
        REMOTE_USER = 'docker'
        REMOTE_HOST = '192.168.5.48'
        REMOTE_TOMCAT_PATH = '/opt/tomcat9/webapps'
    }

    stages {
        stage('Checkout') {
            steps {
                github 'https://github.com/vinver1981/hello-world.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh "${MVN_CMD} clean install"
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // Use Jenkins credentials to securely manage SSH details
                withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key-id', keyFileVariable: 'SSH_KEY')]) {
                    sh """
                        scp -i $SSH_KEY target/${WAR_NAME} ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_TOMCAT_PATH}
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Build or deployment failed.'
        }
    }
}
