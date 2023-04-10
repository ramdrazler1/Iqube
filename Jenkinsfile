pipeline {
    agent {
        label 'Node-1'
    }
    environment {
        BUILD_NAME = 'iqube'
        EXE_PATH = '/home/ubuntu/iqube/workspace/Dev-Test'
    }
    parameters {
        string(name: 'GIT_REPO_URL', description: 'Enter the Git repository URL')
        string(name: 'GIT_BRANCH', description: 'Enter the Git branch name')
    }
    stages {
            stage('Checkout') {
            steps {
                git branch: "${params.GIT_BRANCH}", url: "${params.GIT_REPO_URL}"
            }
        }
//        stage("Git Checkout") {
//            steps {
//                git branch: 'main', credentialsId: 'b53df08d-9aad-42ae-8351-d0b4d2f13a67', url: 'https://github.com/ramdrazler1/Iqube.git'
//            }
//       } 
                 
        stage("Stop and Remove the Existing Container"){
          steps{
             sh '''
              docker stop $BUILD_NAME
              docker rm $BUILD_NAME
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
             sh 'docker run -d --name iqube -p 3000:3000 $BUILD_NAME:latest '
             echo 'Image Running in Container'  
            }
        }          
    }
}
