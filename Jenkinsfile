pipeline {
    agent any
    tools {
        maven '3.9.6'
        jdk 'jdk-9.0.4'
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
                        expression { branch 'master' }
                        expression { branch 'develop' }
                    }
                }
            }
            steps {
                sh "mvn clean compile"
            }
        }
        stage("Run Test cases") {
            when {
                expression { branch 'develop' }
            }
            steps {
                sh "mvn clean test"
            }
        }
        stage("Check Code coverage") {
            when {
                expression { branch 'develop' }
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
                expression { branch 'master' }
            }
            steps {
                sh "mvn tomcat7:deploy"
            }
        }
    }
}
