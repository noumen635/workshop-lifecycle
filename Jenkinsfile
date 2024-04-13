pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/noumen635/workshop-lifecycle.git'
            }
        }

        stage('Code Analysis') {
            steps {
                sh 'Performing code quality checks'
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Automated Testing') {
            steps {
                echo 'Performing automated testing'
            }
        }

        stage('Build Artifact') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t noumendarryl/helloworld:1.${BUILD_NUMBER} --pull --no-cache . '
            }
        }

        stage('Deploy on DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'DOCKER_ID', variable: 'DOCKER_PWD')]) {
                    sh 'docker login -u noumendarryl -p ${DOCKER_PWD}'
                }
                sh 'docker push noumendarryl/helloworld:1.${BUILD_NUMBER}'
                sh 'docker push noumendarryl/helloworld:latest'
            }
        }

        stage('Email Notification') {
            steps {
                emailext body: '$PROJECT_NAME - BUILD # $BUILD_NUMBER - $BUILD_STATUS : Check console output at $BUILD_URL to view results. Please note this is an automated email.',
                subject: '$PROJECT_NAME - BUILD # $BUILD_NUMBER - $BUILD_STATUS',
                to: 'ndrayl70@gmail.com'
            }
        }
    }
}