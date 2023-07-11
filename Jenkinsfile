pipeline {
    agent none
    environment {
        BUILD_NAME = 'iqube'
        EXE_PATH = '/home/ubuntu/iqube/workspace/Dev-Test'
    }
    parameters {
        choice(name: 'AGENT', choices: ['Build-test', 'Sprint', 'Stage', 'UAT'], description: 'Select the Build Environment')
        string(name: 'GIT_REPO_URL', description: 'Enter the Git repository URL')
        string(name: 'GIT_BRANCH', description: 'Enter the Git branch name')
        
    }
    stages {
        stage('Checkout') {
            agent { label "${params.AGENT}" }
            steps {
                git url: "${params.GIT_REPO_URL}", branch: "${params.GIT_BRANCH}"
            }
        }
                 
     stage("Stop and Remove the Existing Container") {
         agent { label "${params.AGENT}" }
      steps {
         script {
            def containerStatus = sh(returnStatus: true, script: "docker inspect -f {{.State.Running}} $BUILD_NAME").trim()
            
            if (containerStatus == 'true') {
                sh 'docker stop $BUILD_NAME'
                sh 'docker rm $BUILD_NAME'
                echo 'Container has been Stopped and Removed'
            } else {
                echo 'Container is not running or does not exist. Skipping...'
            }
        }
    }
}   

        
        stage("Build the Images") {
            agent { label "${params.AGENT}" }
            steps {
                sh "docker build -t $BUILD_NAME:latest $EXE_PATH"
            }
        }   
        stage("Run the Images") {
            agent { label "${params.AGENT}" }
            steps {
                sh 'docker run -d --name iqube -p 3000:3000 $BUILD_NAME:latest '
                echo 'Image Running in Container'  
            }
        } 
//            post {
//        success {
//           emailext body: 'Build success', 
//                     subject: 'Build Notification', 
//                     to: 'ram7540123@gmail.com, karthik96nv@gmail.com'
//        }
//    }
        
    }
}
//        stage("Git Checkout") {
//            steps {
//                git branch: 'main', credentialsId: 'b53df08d-9aad-42ae-8351-d0b4d2f13a67', url: 'https://github.com/ramdrazler1/Iqube.git'
//            }
//       } 
                 
