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
    }
    stages {
        stage('Build & Test Dev') {
            steps {
                script {
                    withCredentials([
                    file(credentialsId: 'gcr-private-repo-reader', variable: 'GCR_KEY')
                    ]) {
                        dir('src'){
                            sh 'echo $GIT_BRANCH'
                            sh "make push_base"
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
