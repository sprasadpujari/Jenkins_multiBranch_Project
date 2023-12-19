pipeline {
    agent any
    tools {
        maven '3.9.6'
        jdk 'java8'
    }
    stages {
        stage("Tools initialization") {
            steps {
                sh "mvn --version"
                sh "java -version"
            }
        }
        stage("Checkout Code") {
            steps {
                checkout scm
            }
        }
        stage("Check Code Health") {
            when {
                not {
                    anyOf {
                        expression { branch 'main' }
                        expression { branch 'dev' }
                    }
                }
            }
            steps {
                sh "mvn clean compile"
            }
        }
        stage("Run Test cases") {
            when {
                expression { branch 'dev' }
            }
            steps {
                sh "mvn clean test"
            }
        }
        stage("Check Code coverage") {
            when {
                expression { branch 'dev' }
            }
            steps {
                jacoco(
                    execPattern: '**/target/**.exec',
                    classPattern: '**/target/classes',
                    sourcePattern: '**/src',
                    inclusionPattern: 'com/iamvickyav/**',
                    changeBuildStatus: true,
                    minimumInstructionCoverage: '30',
                    maximumInstructionCoverage: '80'
                )
            }
        }
        stage("Build & Deploy Code") {
            when {
                expression { branch 'main' }
            }
            steps {
                sh "mvn tomcat7:deploy"
            }
        }
    }
}
