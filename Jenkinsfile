pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello'
            }
        }
        stage('List GCP Clusters') {
            steps {
                withCredentials([file(credentialsId: 'gcp-json-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    sh '''
                        # Authenticate with service account
                        gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS
                        
                        # Set the project you want to use
                        gcloud config set project devopslearning-496304
                        
                        # List all GKE clusters in the project
                        gcloud container clusters list
                        gcloud container clusters get-credentials my-cluster --zone us-central1-a

                        # Verify deployment
                        kubectl get pods -o wide
                        kubectl get svc  -o wide
                       
                    '''
                }
            }
        }

        
    }
}