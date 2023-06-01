pipeline{
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage('Build Maven project'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/krishtambe/java-app']])
                sh 'mvn clean install' 
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t tambekrish/java-app .' 
                }
            }
        }
        stage('Push image to docker hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'pwd-docker-hub', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u tambekrish -p ${dockerhubpwd}'
                    }
                    sh 'docker push tambekrish/java-app'
                }
            }
        }
    }
}