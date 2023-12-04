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
    }
}
