pipeline {
    agent any
    environment { 
        APP_VERSION = '0.0.1'
        IMG_NAME = 'app1'
        DOCKER_REGISTRY = 'https://gcr.io'
        DOCKER_REPO = 'gcr.io/YOUR_REPO'
        ARGOCD_SERVER='cd.domain.tld'
        ARGO_PROJECT='testargo'
        NAMESPACE='argotest'
        AZ_REGISTRY='bdgapp.azurecr.io'
    }
    stages {
        stage('Build & Test Dev') {
            steps {
                script {
                    withCredentials([    
                        usernamePassword(credentialsId: 'azure_credentials', usernameVariable: 'AZ_USER', passwordVariable: 'AZ_PASSWORD')
                    ]) {
                        dir('src'){
                            sh 'echo $GIT_BRANCH'
                            sh "make push_az"
                            sh "make clean"
                        }                        
                    }
                }                
            }
        }
        stage('Deploy with Argo') {
            steps {
                script {
                    withCredentials([
                    string(credentialsId: 'argo_token', variable: 'ARGO_TOKEN'),
                    file(credentialsId: 'gcr-private-repo-reader', variable: 'GCR_KEY')
                    ]) {
                        dir('src'){
                            sh "make deploy"
                        }    
                    }
                }                
            }
        }
    }
}
