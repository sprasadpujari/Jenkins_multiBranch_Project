
pipeline {
    agent any
    tools {
        maven '3.9.6'
        jdk 'java8'
    }
    stages {
        stage("Checkout Code") {
            steps {
                checkout scm
            }
        }
        stage("Check Code Health") {
            when {
                not {
                    anyOf {
                        branch 'main'
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
                branch 'dev'
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
                    maximumInstructionCoverage: '80'
                )
            }
        }
        stage("Build & Deploy Code") {
            when {
                branch 'main'
            }
            steps {
                script{
                    def javaHome = tool 'java8'
                    env.JAVA_HOME = "${javaHome}/bin/java"
             sh """
                echo "Building Artifact........................."
                """

                sh """
                echo "Deploying Code............................."
                """
                     withMaven(maven : '3.9.6') {
                    sh 'mvn deploy'
                    
                }
            }
        }
    }
}
