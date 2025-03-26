pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "ephrash1/frontend-ecomm"
        DOCKER_TAG = "latest"
        EKS_CLUSTER_NAME = "ephrash-cluster"
        KUBECONFIG = credentials('k8s-cred')
    }
    tools{
        maven "Maven"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/Muthaiyyan/vinodses_ecomm_store_Dar.git'
            }
        }
        stage('Docker Image'){
            steps{
             sh 'docker build -t ephrash1/frontend-ecomm .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                    sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'
                }
            }
        }
        stage('Deploy to Kubernetes (Using Local Deployment YAML)') {
            steps {
                sh "kubectl apply -f /var/lib/jenkins/workspace/Project3FE/Deployment.yml"
            }
        } 
      }
    }
