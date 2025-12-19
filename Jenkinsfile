@Library('my-shared-lib') _

pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'rushikeshborekar/freelancerimage:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/rushgit2203/bootstrap.git'
            }
        }

        stage('Build') {
            steps {
                echo ' Compiling project...'
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                runTests()    
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Docker Build') {
            steps {
                dockerBuild("${DOCKER_IMAGE}")   
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${DOCKER_IMAGE}
                    '''
                }
            }
        }
    }

    post {
        success {
            echo ' Build and push completed successfully!'
        }
        failure {
            echo ' Pipeline failed!'
        }
    }
}
