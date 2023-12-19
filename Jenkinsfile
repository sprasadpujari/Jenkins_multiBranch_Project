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
                    // Uncomment and customize if needed
                    // env.MAVEN_OPTS = '-Xmx1024m -XX:MaxPermSize=256m'
                    sh 'mvn clean install'
                }
            }
        }

        stage('Test') {
            steps {
                script {
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
                    // Uncomment and customize the deployment step
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
