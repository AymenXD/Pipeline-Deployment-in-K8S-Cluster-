pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/AymenXD/Pipeline-Deployment-in-K8S-Cluster-.git', branch: 'master'
            }
        }

        stage('Deploy to Minikube') {
            steps {
                echo 'Deploying Kubernetes configurations...'
                kubernetesDeploy(configs: "webapp-deployment.yaml", kubeconfigId: "kubernetes")
                kubernetesDeploy(configs: "webapp-loadbalancer.yaml", kubeconfigId: "kubernetes")
                kubernetesDeploy(configs: "webapp-service.yaml", kubeconfigId: "kubernetes")
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Verifying application deployment...'
                
                // Verify Pods are running
                echo 'Checking if Pods are running...'
                sh 'kubectl get pods'

                // Verify Services are available
                echo 'Checking if Services are available...'
                sh 'kubectl get svc'

                // Verify Load Balancer status
                echo 'Checking Load Balancer configuration...'
                sh 'kubectl get deploy'
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
