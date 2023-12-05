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
        stage('Cleanup Workspace') {
            steps {
                cleanWs() // Clean the workspace before starting
            }
        }

        stage('Checkout from SCM') {
            steps {
                // Checkout the main branch from the specified GitHub repository
                git branch: 'main', 
                    url: 'https://github.com/jal119/registration-app.git', 
                    credentialsId: 'github'
            }
        }

        stage('Build Application') {
            steps {
                // Run Maven clean and package commands
                sh 'mvn clean package'
            }
        }

        stage('Test Application') {
            steps {
                // Execute tests using Maven
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh "${env.SONAR_SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectName=Register-app -Dsonar.java.binaries=. -Dsonar.projectKey=Register-app"
                }
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                }
            }
        }
    }
}
