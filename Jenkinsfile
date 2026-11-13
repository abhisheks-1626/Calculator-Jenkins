
pipeline {
    agent any

    tools {
        jdk 'JDK 25'
        maven 'Maven-3.10'
    }

    stages {

        stage('SCM Synchronization') {
            steps {
                echo 'Pulling latest code...'
                checkout scm
            }
        }

        stage('Static Verification') {
            steps {
                echo 'Checking Maven project...'
                bat 'mvn validate'
            }
        }

        stage('Compilation and Testing') {
            steps {
                echo 'Running Maven tests...'
                bat 'mvn clean package'
            }
        }
    }

    post {
        always {
            junit testResults: '**/target/surefire-reports/*.xml',
                  allowEmptyResults: true
        }

        success {
            echo 'Build Successful!'
        }

        failure {
            echo 'Build Failed!'
        }
    }
}
