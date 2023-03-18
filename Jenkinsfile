pipeline {
    agent any 
    environment {
        dockerhub=credentials('docker-venuchanapathi1998')
        TOKEN_NAME='myacrtoken'
        TOKEN_PWD='ny7IaZ1pY0RzwWhLRleB8WPw4fd7/NNvSKtDiuEmux+ACRDKYa66'
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
        stage('Authenticate with azure ACR') {
            steps {
                sh 'echo $TOKEN_PWD | docker login --username $TOKEN_NAME --password-stdin nginxappmine.azurecr.io'
            }
        }
        stage('cleaning the loaded images') {
            steps {
                sh 'docker rmi -f $(docker images -q)'
            }
        }
    }
}
