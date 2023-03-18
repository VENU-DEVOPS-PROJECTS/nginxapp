pipeline {
    agent any 
    environment {
        dockerhub=credentials('docker-venuchanapathi1998')
    }
    stages {
        stage('Clone') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/VENU-DEVOPS-PROJECTS/nginxapp.git']]])
            }
        }
        stage('listing files') {
            steps {
                sh 'ls -l'
            }
        }
        stage('Docker version') {
            steps {
                sh 'docker --version'
            }
        }
        stage('Building docker image from Dockerfile') {
          steps {
                sh 'docker build -t nginxappmine:${BUILD_NUMBER} .'
            }
        }
        stage('Tagging image') {
           steps {
               sh 'docker tag nginxappmine:${BUILD_NUMBER} venuchanapathi1998/nginxappmine:${BUILD_NUMBER}'
           }
        }
        stage('Logging in') {
            steps {
                sh 'echo $dockerhub_PSW | docker login -u $dockerhub_USR --password-stdin'
            }
        }
        stage('Pushing to docker hub') {
            steps {
                sh 'docker push venuchanapathi1998/nginxappmine:${BUILD_NUMBER}'
            }
        }
        stage('Pushing to ECR') {
            steps {
                sh 'docker tag nginxappmine:${BUILD_NUMBER} public.ecr.aws/c4r1v1f4/nginxappmine:${BUILD_NUMBER}'
                sh 'docker push public.ecr.aws/c4r1v1f4/nginxappmine:${BUILD_NUMBER}'
            }
        }
        stage('Creating the Docker container from the Docker image ceated in previous stage') {
          steps {
                sh ' docker run -d --name clockapp nginxappmine:${BUILD_NUMBER}'
            }
        }
        stage('cleaning the loaded images') {
            steps {
                sh 'docker stop $(docker ps -aq)'
                sh 'docker rm $(docker ps -aq)'
                sh 'docker rmi -f $(docker images -q)'
            }
        }
    }
}
