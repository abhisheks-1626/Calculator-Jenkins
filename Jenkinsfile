pipeline {
    agent any

    tools {
        jdk 'JDK 25'
        maven 'Maven-3.10'
    }

    stages {
        stage('SCM Synchronization') {
            steps {
                echo '📥 Pulling latest code changes from Git repository...'
                checkout scm
            }
        }

        stage('Static Verification') {
            steps {
                echo '🔍 Checking Maven project layout rules...'
                bat 'mvn validate'
            }
        }

        stage('Compilation & Automated Testing') {
            steps {
                echo '⚙️ Compiling class layers and verifying test logic stability...'
                bat 'mvn clean package'
            }
            post {
                always {
                    echo '📊 Publishing Unit Test trends to Jenkins Dashboard...'
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Starting SonarQube analysis'
                withSonarQubeEnv('SonarQube-Local') {
                    bat 'mvn sonar:sonar -Dsonar.token=%SONAR_AUTH_TOKEN%'
                }
            }
        }
    }

    post {
        success {
            echo 'Build and SonarQube analysis completed!'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
