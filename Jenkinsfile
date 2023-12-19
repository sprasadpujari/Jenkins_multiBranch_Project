pipeline {
    agent any
    tools {
        maven '3.9.6'
        jdk 'java8'
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
                        branch 'main';
                        branch 'dev'
                    }
                }
           }
           steps {
               sh "mvn clean compile"
            }
        }
        stage("Run Test cases") {
            when {
                branch 'dev';
            }
           steps {
               sh "mvn clean test"
            }
        }
        stage("Check Code coverage") {
            when {
                branch 'dev'
            }
            steps {
               jacoco(
                    execPattern: '**/target/**.exec',
                    classPattern: '**/target/classes',
                    sourcePattern: '**/src',
                    inclusionPattern: 'com/iamvickyav/**',
                    changeBuildStatus: true,
                    minimumInstructionCoverage: '30',
                    maximumInstructionCoverage: '80')
           }
        }
        stage("Build & Deploy Code") {
            when {
                branch 'main'
            }
            steps {
                sh "mvn tomcat7:deploy"
            }
        }
    }
 }
