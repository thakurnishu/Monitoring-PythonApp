def COLOR_MAP = [
    'SUCCESS': 'good',
    'FAILURE': 'danger'
]

def dockerImage

pipeline {
    agent any

    environment {
        docker_registry = "nishant0510/pythonapp"
        docker_registryCredential = 'docker_id'
        imageTag = "v${env.BUILD_ID}"
    }
    
    stages {

        stage('Fetch code') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/thakurnishu/Monitoring-PythonApp.git']]])
            }
        }

        stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build "${docker_registry}:${imageTag}"
                    dockerLatestImage = docker.build "${docker_registry}:latest"
                }
                
            }
        }

        stage('Push image') {
            steps {
                withCredentials([usernamePassword(credentialsId: docker_registryCredential, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        sh 'docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}'
                        sh 'docker push ${docker_registry}:${imageTag}'
                        sh 'docker push ${docker_registry}:latest'
                    }
                }
                
                sh 'docker rmi ${docker_registry}:${imageTag}'
                sh 'docker rmi ${docker_registry}:latest'
            }
        }

        // stage('Running Image in Container instances') {
        //     steps {
        //         sh '''
        //         chmod +x BashScript.sh
        //         ./BashScript.sh
        //         '''
        //     }
        // }
    }
}