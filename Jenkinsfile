pipeline {
    agent any  // Use any agent, assuming the global configuration uses jenkins-docker-kubectl

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/AymenXD/Pipeline-Deployment-in-K8S-Cluster-.git', branch: 'master'
            }
        }

        stage('Deploy to Minikube') {
            steps {
                echo 'Applying Kubernetes configurations...'
                sh 'ls -la'  // Verify YAML files are present in the workspace
                sh 'kubectl apply -f webapp-deployment.yaml --validate=false'
                sh 'kubectl apply -f webapp-loadbalancer.yaml --validate=false'
                sh 'kubectl apply -f webapp-service.yaml --validate=false'
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Checking pods...'
                sh 'kubectl get pods -l app=webapp'

                echo 'Checking services...'
                sh 'kubectl get svc -l app=webapp'

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
