pipeline {
    agent any
    environment {
        BUILD_NAME = 'iqube'
        PATH = '/var/jenkins_home/workspace/Dev-Test/'
    }
    stages {
        stage("Git Checkout") {
            steps {
                git branch: 'main', credentialsId: 'b53df08d-9aad-42ae-8351-d0b4d2f13a67', url: 'https://github.com/ramdrazler1/Iqube.git'
            }
        }  
        stage("Build the Images") {
            steps {
                sh "docker build -t $BUILD_NAME:latest $PATH"
            }
        }  
    }
}
