pipeline {
    agent {
        label 'Jenkins-Agent'
    }

    tools {
        jdk 'Java17'
        maven 'maven3'
    }

    environment {
        SONAR_SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/jal119/registration-app.git', credentialsId: 'github'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
                // List directory contents for diagnostic purposes
                sh 'ls -alh'
            }
        }

        stage('Test Application') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Code Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    // Ensure that the path to 'target' is correct
                    sh script: "${env.SONAR_SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectName=Register-app -Dsonar.java.binaries=target/classes -Dsonar.projectKey=Register-app"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                }
            }
        }
    }
    post {
        always {
            // Clean up and finalize steps here
        }
    }
}
