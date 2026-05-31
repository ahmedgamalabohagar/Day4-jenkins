@Library('shared-lib') _

pipeline {
    agent{
        label 'agent01'
    }
    tools {
        jdk 'jdk-11.0.2'
        maven 'maven-354'
    }

    environment {
        dockerUsername = credentials('docker-username')
        dockerPassword = credentials('docker-password')
    }

    stages {
        stage('Build Java App') {
            steps {
                script {
                    def mvn = new edu.iti.mavenClass()
                    mvn.build('clean package -DskipTests')
                }
            }
        }

        stage('Archive Java App') {
            steps {
                archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
            }
        }

        stage('Test Java App') {
            steps {
                script {
                    def mvn = new edu.iti.mavenClass()
                    mvn.test('')
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def dockerObj = new edu.iti.dockerClass()
                    dockerObj.build('ahmedabohagar/java-app', "v${env.BUILD_ID}")
                }
            }
        }

        stage('Docker Login & Push') {
            steps {
                script {
                    def dockerObj = new edu.iti.dockerClass()
                    dockerObj.login(env.dockerUsername, env.dockerPassword)
                    dockerObj.push('ahmedabohagar/java-app', "v${env.BUILD_ID}")
                }
            }
        }
    }
}


