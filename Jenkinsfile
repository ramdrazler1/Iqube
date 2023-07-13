pipeline {
    agent none
    environment {
        BUILD_NAME = 'iqube'
        EXE_PATH = '/home/ubuntu/jenkins-slave/workspace/DevOps/Declarative-demo/'
        NEXUS_REGISTRY_URL = 'https://spanartifacts.spanllc.com/'
        NEXUS_REPOSITORY = 'test-docker'
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
                git url: "${params.GIT_REPO_URL}, branch: "${params.GIT_BRANCH}""
            }
        }

        stage("Stop and Remove the Existing Container") {
            agent { label "${params.AGENT}" }
            steps {
                sh '''
                    docker stop $BUILD_NAME
                    docker rm $BUILD_NAME
                '''
                echo 'Container has been Stopped and Removed'
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
                sh "docker run -d --name $BUILD_NAME -p 3000:3000 $BUILD_NAME:latest"
                echo 'Image Running in Container'
            }
        }

        stage('Publish') {
            agent { label "${params.AGENT}" }
            steps {
                // Login to Nexus Docker registry
                sh 'docker login -u devopsadmin -p SpanTEc$@2023 $NEXUS_REGISTRY_URL'

                // Tag the Docker image with Nexus repository URL
                sh 'docker tag $BUILD_NAME:latest $NEXUS_REGISTRY_URL/$NEXUS_REPOSITORY/$BUILD_NAME:latest'

                // Push the Docker image to Nexus repository
                sh 'docker push $NEXUS_REGISTRY_URL/$NEXUS_REPOSITORY/$BUILD_NAME:latest'
            }
        }
    }
}
//            post {
//        success {
//           emailext body: 'Build success', 
//                     subject: 'Build Notification', 
//                     to: 'ram7540123@gmail.com, karthik96nv@gmail.com'
//        }
//    }
        
//    }
//}
//        stage("Git Checkout") {
//            steps {
//                git branch: 'main', credentialsId: 'b53df08d-9aad-42ae-8351-d0b4d2f13a67', url: 'https://github.com/ramdrazler1/Iqube.git'
//            }
//       } 
                 
