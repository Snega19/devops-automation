pipeline {
    agent any
    environment {
        dockerHubCredential = 'dockerhub'
    }
    tools{
        maven 'maven_3_5_0'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Snega19/devops-automation.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t snega19/devops-automation .'
                }
            }
        }
        stage('Pushing Image') {
    steps {
        script {
            docker.withRegistry('https://registry.hub.docker.com', dockerHubCredential) {
                sh 'docker push snega19/devops-automation'
            }
        }
    }
}

        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'Kubernetes')
                }
            }
        }
    }
}
