pipeline {
    agent any
    triggers {
        pollSCM('*/5 * * * *')
    }
    options { disableConcurrentBuilds() }
    stages {
        stage('Permissions') {
            steps {
                sh 'chmod 775 *'
            }
        }
        stage('Cleanup') {
                steps {
                    sh './mvnw clean'
                }
            }
        
        stage('Test') {
            steps {
                sh './mvnw test'
            }
            post {
                always {
                    junit allowEmptyResults:true, testResults: 'build/test-results/test/*.xml'
                }
            }
        }
        stage('Build') {
            steps {
                sh './mvnw package'
            }
        }
    }
}