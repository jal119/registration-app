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
        stage('Prepare') {
            steps {
                cleanWs() // Clean the workspace before starting
            }
        }

        stage('Checkout Code') {
            steps {
                // Checkout the main branch from the specified GitHub repository
                git branch: 'main', 
                    url: 'https://github.com/jal119/registration-app.git', 
                    credentialsId: 'github'
            }
        }

        stage('Build') {
            steps {
                // Run Maven clean and package commands
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                // Execute tests using Maven
                sh 'mvn test'
            }
        }

        stage('Code Quality Analysis') {
            steps {
                // Run SonarQube scanner
                withSonarQubeEnv('sonar-server') {
                    sh "${env.SONAR_SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectName=webapp -Dsonar.java.binaries=. -Dsonar.projectKey=webapp"
                }

                // Check the quality gate status
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                }
            }
        }
    }
}
