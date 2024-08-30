pipeline {
    agent {
        kubernetes {
            label 'jenkins-agent'
            yaml """
            apiVersion: v1
            kind: Pod
            spec:
              containers:
              - name: kubectl
                image: bitnami/kubectl:latest
                command:
                - cat
                tty: true
            """
        }
    }

    environment {
        KUBE_CONFIG = credentials('kube-config')
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Putumerta-collab/test_k8s.git', branch: 'main'
            }
        }

        stage('Install kubectl') {
            steps {
                sh '''
                    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                    chmod +x kubectl
                    mv kubectl /usr/local/bin/
                '''
            }
        }

        stage('Deploy NGINX') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'kube-config-id', variable: 'KUBECONFIG')]) {
                        sh 'kubectl apply -f k8s/nginx-deployment.yaml'
                        sh 'kubectl apply -f k8s/nginx-service.yaml'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment and Service applied successfully!'
        }
        failure {
            echo 'There was an issue with deploying NGINX!'
        }
    }
}
