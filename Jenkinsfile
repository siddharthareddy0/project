pipeline {
    agent any

    tools {
        jdk 'jdk17'        // Jenkins tool name
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'gradlew clean :app:build'
            }
        }

        stage('Test') {
            steps {
                bat 'gradlew :app:test'
            }
            post {
                always {
                    junit 'app/build/test-results/test/*.xml'
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'app/build/libs/*.jar', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                bat 'wsl ansible-playbook deploy.yml'
            }
        }
    }
}
