pipeline {
    agent any
    tools{
        maven 'maven'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'git@github.com:pavan2619/devops-automation.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t pavan131/kubernetes .'
                }
            }
        }
        stage('Push image to hub') {
    steps {
        script {
            withCredentials([string(credentialsId: '9985819131', variable: 'DOCKERHUB_PASSWORD')]) {
                sh "echo ${DOCKERHUB_PASSWORD} | docker login -u pavan131 --password-stdin"
            }
            sh 'docker push pavan131/kubernetes'
        }
    }
}

        stage('Deploy to K8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubeconfig')
                }
            }
        }
    
    }    
}
