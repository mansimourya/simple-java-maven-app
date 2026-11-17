
pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'mvn -B clean compile'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn -B test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                bat 'mvn -B package'
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                    if not exist target\\*.jar (
                        echo No JAR file found.
                        exit /b 1
                    )

                    if not exist deploy mkdir deploy

                    copy /Y target\\*.jar deploy\\
                '''
            }
        }
    }
}
