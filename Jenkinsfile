pipeline {
    agent any

    stages {
        stage('Connect to GKE') {
            steps {
                // Use a GCP service account JSON key stored in Jenkins credentials
                withCredentials([file(credentialsId: 'gcp-json-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    sh '''
                        echo "Activating service account..."
                        gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS
                        echo "Setting project and zone..."
                        gcloud config set project devopslearning-496304
                        gcloud config set compute/zone us-central1-a
                        # Required for GKE auth plugin
                        export USE_GKE_GCLOUD_AUTH_PLUGIN=True
                        gcloud container clusters get-credentials my-cluster \
                                --zone us-central1-a\
                                --project devopslearning-496304
                        
                     '''
                }
            }
        }
        stage('List GKE Nodes') {
            steps {
                sh '''
                    export USE_GKE_GCLOUD_AUTH_PLUGIN=True
                    kubectl get nodes
                    kubectl get pods
                '''
            }
        }
    }
}
