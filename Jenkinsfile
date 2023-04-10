pipeline {
    agent {
        label 'Node-1'
    }
    environment {
        BUILD_NAME = 'iqube'
        EXE_PATH = '/home/ubuntu/iqube/workspace/Dev-Test'
    }
    stages {
        stage("Git Checkout") {
            steps {
                git branch: 'main', credentialsId: 'b53df08d-9aad-42ae-8351-d0b4d2f13a67', url: 'https://github.com/ramdrazler1/Iqube.git'
            }
        } 
                 
        stage("Stop and Remove the Existing Container"){
        steps{
           sh '''
           docker stop $BUILD_NAME:latest
           docker rm $BUILD_NAME:latest
           '''
            echo 'Container has been Stopped and Removed'  
            }
        }      

        
        stage("Build the Images") {
            steps {
                sh "docker build -t $BUILD_NAME:latest $EXE_PATH"
            }
        }   
        stage("Run the Images") {
            steps {
                sh "docker run -itd -p 8001:80 $BUILD_NAME:latest"
            }
        }          
        
    }
}
