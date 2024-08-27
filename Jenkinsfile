pipeline {
    agent any
    
    environment {
        DOCKER_ID = "jprianon"
        DOCKER_IMAGE = "carts-service"  // Nom de l'image Docker spécifique au microservice
        DOCKER_TAG = "v.${BUILD_ID}.0"
        KUBECONFIG = "/home/ubuntu/.kube/config"
        CHART_VERSION = '1.0.0'
    }
    
    stages {
        stage('Docker Build') {
            steps {
                script {
                    sh '''
                    ls
                    git clone https://github.com/jprianon/carts.git
                    docker rmi -f ${DOCKER_ID}/${DOCKER_IMAGE}
                    docker build -t ${DOCKER_ID}/${DOCKER_IMAGE} .
                    '''
                }
            }
        }
        
        stage('Push') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDENTIALS_ID) {
                        dockerImage.push("${env.BUILD_ID}")
                        dockerImage.push('latest')
                    }
                }
            }
        }

        //stage('Deploy to Dev') {
        //    environment {
        //        KUBECONFIG = credentials("config")
        //    }
        //    steps {
        //        script {
        //            sh '''
        //            rm -Rf .kube
        //            mkdir .kube
        //            cat $KUBECONFIG > .kube/config
        //            helm upgrade --install carts-service-dev helmchart --namespace dev --values helmchart/values-dev.yaml
        //            kubectl get deploy,svc,Pod -n dev
        //            '''
        //        }
        //    }
        //}
//
        //stage('Deploy to QA') {
        //    environment {
        //        KUBECONFIG = credentials("config")
        //    }
        //    steps {
        //        script {
        //            sh '''
        //            rm -Rf .kube
        //            mkdir .kube
        //            cat $KUBECONFIG > .kube/config
        //            helm upgrade --install carts-service-qa helmchart --namespace qa --values helmchart/values-qa.yaml
        //            kubectl get deploy,svc,Pod -n qa
        //            '''
        //        }
        //    }
        //}
//
        //stage('Deploy to Staging') {
        //    environment {
        //        KUBECONFIG = credentials("config")
        //    }
        //    steps {
        //        script {
        //            sh '''
        //            rm -Rf .kube
        //            mkdir .kube
        //            cat $KUBECONFIG > .kube/config
        //            helm upgrade --install carts-service-staging helmchart --namespace staging --values helmchart/values-staging.yaml
        //            kubectl get deploy,svc,Pod -n staging
        //            '''
        //        }
        //    }
        //}
//
        //stage('Manual Deployment to Production') {
        //    when {
        //        expression {
        //            env.GIT_BRANCH == "main"
        //        }
        //    }
        //    steps {
        //        input message: 'Deploy to production environment?', ok: 'Yes'
        //        script {
        //            sh '''
        //            rm -Rf .kube
        //            mkdir .kube
        //            cat $KUBECONFIG > .kube/config
        //            helm upgrade --install carts-service-prod helmchart --namespace prod --values helmchart/values-prod.yaml
        //            kubectl get deploy,svc,Pod -n prod
        //            '''
        //        }
        //    }
        //}
    }
}
