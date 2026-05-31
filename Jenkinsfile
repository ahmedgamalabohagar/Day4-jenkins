pipeline {

    agent {
        label 'agent01'
    }

    tools {
        jdk 'jdk-11'
        maven 'maven-354'
    }

    environment {
        dockerUsername = credentials('docker-username')
        dockerPassword = credentials('docker-password')
    }

    stages {

        stage('Build Java App') {
            steps {
                sh 'mvn package install -DskipTests'
            }
        }

        stage('Archive Java App') {
            steps {
                archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t java-app:v1 .'
            }
        }

        stage('Docker Login') {
            steps {
                sh 'docker login -u ${dockerUsername} -p ${dockerPassword}'
            }
        }

        // stage('Push Docker Image') {
        //     steps {
        //         sh 'docker push java-app:v1'
        //     }
        // }

    }
}
