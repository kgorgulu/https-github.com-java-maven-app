#!/usr/bin/env groovy

pipeline {
tools {
maven 'maven-3.8'
}
    agent none
    stages {
        stage('build jar') {
            steps {
                script {
                    echo "Building the application..."
                    ssh 'mvn package'
                }
            }
        }
        stage('build image') {
            steps {
                script {
                    echo "Building the docker image..."
                     withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER' ) ])
                             sh 'docker build -t ratalay35/my-repo:jma-2.2 .'
                             sh "echo $PASS | docker login -u $USER --password-stdin"
                             sh 'docker push ratalay35/my-repo:jma-2.2 '
                }                                                                                          
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo "Deploying the application..."
                    echo "Building version ${SEVER_SERVER_CREDENTIALS}"
                    sh "${SEVER_SERVER_CREDENTIALS}"
                }
            }
        }
    }
}
