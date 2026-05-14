pipeline {
    agent any

    environment {
        PROJECT_ID   = 'devopslearning-496304'
        CLUSTER_NAME = 'my-cluster'
        CLUSTER_ZONE = 'us-central1-a'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Authenticate GCP') {
            steps {
                withCredentials([file(credentialsId: 'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    sh '''
                    gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS
                    gcloud config set project $PROJECT_ID
                    gcloud container clusters get-credentials $CLUSTER_NAME --zone $CLUSTER_ZONE
                    '''
                }
            }
        }

        stage('Deploy to GKE') {
            steps {
                sh '''
                kubectl apply -f deployment.yml
                kubectl apply -f LoadBalancer-service.yml
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                kubectl rollout status deployment/nginx-deploy
                kubectl get svc
                '''
            }
        }
    }
}