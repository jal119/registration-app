pipeline {
    agent { label 'Jenkins-Agent' }

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
                git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/jal119/registration-app.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
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
                    sh "${env.SONAR_SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectName=Register-app -Dsonar.java.binaries=target -Dsonar.projectKey=Register-app"
                }
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
    }
}
