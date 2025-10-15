pipeline {
    agent any

    stages {
        stage('Pull Code From GitHub') {
            steps {
                git 'https://github.com/DevopsNexus-n8n/python_devops.git'
            }
        }
        stage('Build the Docker image') {
            steps {
                sh 'sudo docker build -t weekendimage /var/lib/jenkins/workspace/python-kuberneties'
                sh 'sudo docker tag weekendimage 969815/weekendimage:latest'
                sh 'sudo docker tag weekendimage 969825/weekendimage:${BUILD_NUMBER}'
            }
        }
        stage('Push the Docker image') {
            steps {
                sh 'sudo docker image push 969815/weekendimage:latest'
                sh 'sudo docker image push 969815/weekendimage:${BUILD_NUMBER}'
            }
        }
        stage('Deploy on Kubernetes') {
            steps {
                sh 'sudo kubectl apply -f /var/lib/jenkins/workspace/python-kuberneties/pod.yaml'
                sh 'sudo kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}