pipeline {
    agent {
        kubernetes {
            //label 'docker-build-agent'
            defaultContainer 'jnlp'
            cloud 'kubernetes'
            yaml """
apiVersion: v1
kind: Pod
spec:
  serviceAccountName: jenkins-sa
  containers:
  - name: dind
    image: docker:dind
    securityContext:
      privileged: true
    args:
    - --storage-driver=overlay2
  - name: tools
    image: alpine/helm:3.14.2
    tty: true
    command:
    - sleep
    - 10000
"""
        }
    }

    environment {
        DOCKER_REGISTRY_USER = 'bassant41656'
        APP_NAMESPACE = 'dev'
        HELM_RELEASE_NAME = '3tier-app-release' 
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        DOCKER_CRED_ID = 'docker-hub-credentials'
    }

    stages {
        stage("Source Code") {
            steps {
                echo "Checking source code..."
                git branch: 'HelmCI/CD', url: 'https://github.com/Bassantmohy/three-tier-app-project/'
            }
        }

        stage("Build Backend") {
        steps {
            // Must run inside the 'dind' container to access Docker daemon
            container('dind') {
                echo "Building Backend image with tag: ${IMAGE_TAG}"
                dir('backend/') { 
                    sh "docker build -t ${DOCKER_REGISTRY_USER}/backend-image:${IMAGE_TAG} ."
                }
                echo "Backend image built successfully."
            }
        }
    }
    
    // Stage C: Push ONLY the Backend Image
    stage('Push Backend') {
        steps {
            // Use defined credentials to login securely
            withCredentials([usernamePassword(credentialsId: DOCKER_CRED_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                container('dind') {
                    // Login, push the new backend image, and logout
                    sh "echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin"
                    sh "docker push ${DOCKER_REGISTRY_USER}/backend-image:${IMAGE_TAG}"
                    sh "docker logout"
                }
            }
        }
    }
        

        stage("Deploy with Helm") {
            steps {
                container('tools') {
                    sh """
                    helm upgrade --install $HELM_RELEASE_NAME ./3-tier-app \
                        --namespace $APP_NAMESPACE \
                        --values ./3-tier-app/values.yaml
                    """
                }
            }
        }

        stage('Wait for Pods Ready') {
            steps {
                container('tools') {
                    sh """
                    kubectl wait --for=condition=ready pod -l app=backend-deployment -n ${NAMESPACE} --timeout=220s
                    kubectl wait --for=condition=ready pod -l app=mysql-deployment -n ${NAMESPACE} --timeout=220s
                    kubectl wait --for=condition=ready pod -l app=proxy-deployment -n ${NAMESPACE} --timeout=220s
                    """
                }
            }
        }

        stage("Smoke Test") {
            steps {
                container('tools') {
                    sh """
                    curl -k -s -o /dev/null -w "%{http_code}" https://proxy-svc.${APP_NAMESPACE}.svc.cluster.local/health | grep 200
                    """
                }
            }
        }

    }


    post {
        success {
            mail to: 'bassant16.mohy@gmail.com',
                 subject: "Pipeline Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Pipeline ${env.JOB_NAME} build #${env.BUILD_NUMBER} succeeded.\n${env.BUILD_URL}"
        }
        failure {
            mail to: 'bassant16.mohy@gmail.com',
                 subject: "Pipeline Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Pipeline ${env.JOB_NAME} build #${env.BUILD_NUMBER} failed.\n${env.BUILD_URL}"
        }
    }
}
