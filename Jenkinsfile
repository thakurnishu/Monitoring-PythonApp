def COLOR_MAP = [
    'SUCCESS': 'good',
    'FAILURE': 'danger'
]

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
                    def dockerImage = docker.build "${docker_registry}:${imageTag}"
                }
                
            }
        }

        stage('Push image') {
            steps {
                withCredentials([usernamePassword(credentialsId: docker_registryCredential, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        dockerImage.push()
                        dockerImage.push 'latest'
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