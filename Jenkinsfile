
pipeline {
    agent any

    // Jenkins configured tools
    tools {
        jdk 'JDK 25'
        maven 'Maven-3.10'
    }

    stages {

        // Stage 1: Git Synchronization
        stage('SCM Synchronization') {
            steps {
                echo 'Pulling latest code changes from Git repository...'
                checkout scm
            }
        }

        // Stage 2: Maven Validation
        stage('Static Verification') {
            steps {
                echo 'Checking Maven project layout rules...'
                bat 'mvn validate'
            }
        }

        // Stage 3: Compilation and Unit Testing
        stage('Compilation & Automated Testing') {
            steps {
                echo 'Compiling and running automated tests...'
                bat 'mvn clean package'
            }

            post {
                always {
                    echo 'Publishing Unit Test results...'
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        // Stage 4: SonarQube Code Analysis
        stage('SonarQube Analysis') {
            steps {
                echo 'Starting SonarQube analysis...'

                withSonarQubeEnv('SonarQube-Local') {

                    withCredentials([
                        string(
                            credentialsId: 'sonarqube-token1',
                            variable: 'SONAR_TOKEN'
                        )
                    ]) {

                        bat 'mvn sonar:sonar'
                    }
                }
            }
        }
    }

    // Pipeline Result
    post {
        success {
            echo 'SUCCESS: Build, Testing and SonarQube Analysis completed!'
        }

        failure {
            echo 'FAILURE: Pipeline failed. Check Console Output.'
        }

        always {
            echo 'Jenkins Pipeline execution completed.'
        }
    }
}
