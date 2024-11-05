pipeline {
    agent {
        kubernetes {
            label 'k8s'
        }
    }
    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/AymenXD/Pipeline-Deployment-in-K8S-Cluster-.git', branch: 'master'
            }
        }

        stage('Deploy to Minikube') {
            steps {
                echo 'Applying Kubernetes configurations...'
                // Assuming deployment YAML files are in the repo’s root directory
                sh 'kubectl apply -f webapp-deployment.yaml'
                sh 'kubectl apply -f webapp-loadbalancer.yaml'
                sh 'kubectl apply -f webapp-service.yaml'
            }
        }

        stage('Verify Deployment') {
            steps {
                // Verify the pods are running
                echo 'Checking pods...'
                sh 'kubectl get pods -l app=webapp'

                // Verify the services are active
                echo 'Checking services...'
                sh 'kubectl get svc -l app=webapp'

                // Check the load balancer status
                echo 'Checking load balancer...'
                sh 'kubectl describe svc webapp-loadbalancer'
            }
        }
    }
    post {
        success {
            echo 'Application deployed and verified successfully!'
        }
        failure {
            echo 'Deployment or verification failed.'
        }
    }
}
