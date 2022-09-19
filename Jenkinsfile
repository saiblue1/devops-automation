pipeline {
    agent any
    environment {
        PATH = "/opt/apache-maven-3.8.6/bin:$PATH"
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/saiblue1/devops-automation']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t madhublue01/frontendapplication .'
                }
            }
         }
         stage('Push image to Hub'){
             steps{
                 script{
                     withCredentials([string(credentialsId: 'dockerhub_login', variable: 'dockerhublogin')]) {
                     sh 'docker login -u madhublue01 -p ${dockerhublogin}'

}
                     sh 'docker push madhublue01/frontendapplication'

                 }
             }
         }
         stage('Deploying App to Kubernetes'){
             steps{
                 script{
                     kubernetesDeploy (configs: 'deploymentservice.yml2', kubeconfigId: 'k8config')
                 }
             }
         }
    }
}
