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
        stage('Docker HUB Update') {
            when { branch "master" }
            steps {
                sh '''
                    docker login -u hnizamoglu -p nizam587
                    docker build --no-cache -t aws-backend-test .
                    docker tag aws-backend-test:latest hnizamoglu/aws-backend-test:${TAG_NAME}
                    docker push hnizamoglu/aws-backend-test:${TAG_NAME}
                '''
            }
        }
    }
}