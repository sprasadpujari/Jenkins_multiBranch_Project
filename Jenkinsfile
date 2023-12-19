   pipeline {
    agent any

    tools {
        maven '3.9.6'
        jdk 'java8'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Set the MAVEN_OPTS environment variable if needed
                    // env.MAVEN_OPTS = '-Xmx1024m -XX:MaxPermSize=256m'
                    sh 'mvn clean install'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests, if applicable
                    sh 'mvn test'
                }
            }
        }

        stage('Deploy') {
            when {
                expression { branch 'main' }
            }
            steps {
                script {
                    // Perform deployment steps, if needed
                    // For example, deploy to an application server
                    // sh 'mvn tomcat7:deploy'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
