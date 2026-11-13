pipeline {
    agent any

    tools {
        jdk 'JDK 25'
        maven 'Maven-3.10'
    }

    stages {
        stage('SCM Synchronization') {
            steps {
                checkout scm
            }
        }

        stage('Static Verification') {
            steps {
                bat 'mvn validate'
            }
        }

        stage('Compilation & Automated Testing') {
            steps {
                bat 'mvn clean package'
            }
            post {
                always {
                    junit allowEmptyResults: true,
                          testResults: '**/target/surefire-reports/*.xml'
                }
            }
        }
    }

    post {
        success {
            echo 'Build Successful!'
        }
        failure {
            echo 'Build Failed!'
        }
    }
}