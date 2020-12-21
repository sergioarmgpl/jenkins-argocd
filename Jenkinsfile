pipeline {
    agent any
    environment { 
        APP_VERSION = '0.0.1'
        IMG_NAME = 'app1'
        DOCKER_REGISTRY = 'https://gcr.io'
        DOCKER_REPO = 'gcr.io/YOUR_REPO'
        ARGOCD_SERVER='cd.domain.tld'
        ARGO_PROJECT='testargo'
        NAMESPACE='dev'
        AZ_REGISTRY='bdgapp.azurecr.io'
        DOMAIN='app.demo.pruebasbdg.tk'
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
                        dir('src'){
                            sh "make deploy_helm"
                        }    
                }                
            }
        }
    }
}
