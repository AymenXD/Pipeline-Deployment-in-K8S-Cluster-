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
                kubernetesDeploy(configs: "webapp-deployment.yaml", kubeconfigId: "kubernetes")
                kubernetesDeploy(configs: "webapp-loadbalancer.yaml", kubeconfigId: "kubernetes")
                kubernetesDeploy(configs: "webapp-service.yaml", kubeconfigId: "kubernetes")
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
