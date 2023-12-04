pipeline {
    agent { label 'Jenkins-Agent'}

    tools {
        jdk 'Java17'
        maven 'maven3'
    }

    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/jal119/registration-app.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
            }
        }
          stage('Tests Application') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Code Analysis') {
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
