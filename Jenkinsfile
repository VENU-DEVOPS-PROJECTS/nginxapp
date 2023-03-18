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
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    accessKeyVariable: 'AKIAZH3OJORCJNR7Y64J',
                    secretKeyVariable: '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',
                    credentialsId: 'AWS'
                ]]) {
                sh 'eval $(aws ecr get-login --no-include-email --region us-east-1)'
                sh 'docker tag nginxappmine:${BUILD_NUMBER} 635349595204.dkr.ecr.us-east-1.amazonaws.com/nginxappmine:${BUILD_NUMBER}'
                sh 'docker push 635349595204.dkr.ecr.us-east-1.amazonaws.com/nginxappmine:${BUILD_NUMBER}'
                }
            }
        }
        stage('Authenticate with ACR and PUSHING') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'my-acr-creds', usernameVariable: 'nginxappmine', passwordVariable: 'so+8uAWvCvnDVBU9AxZFsgFNdZu3DP9h7LeWI2gTdB+ACRCAw2BS')]) {
                    sh 'docker login -u $ACR_USERNAME -p $ACR_PASSWORD nginxappmine.azurecr.io'
                    sh 'docker tag nginxappmine:${BUILD_NUMBER} nginxappmine.azurecr.io/nginxappmine:${BUILD_NUMBER}
                    sh 'docker push nginxappmine.azurecr.io/nginxappmine:${BUILD_NUMBER}'
        }
    }
}
        stage('cleaning the loaded images') {
            steps {
                sh 'docker rmi -f $(docker images -q)'
            }
        }
    }
}
