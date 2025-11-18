// Define the Pod Template required for building and deploying (JNLP, DIND, Tools)
podTemplate(label: 'docker-build-agent', containers: [
    containerTemplate(name: 'jnlp', image: 'jenkins/inbound-agent:4.14.0-1-jdk17', command: '/bin/sh', args: '-c cat', tty: true),
    containerTemplate(name: 'dind', image: 'docker:24.0.7-dind', args: '--storage-driver=overlay2', securityContext: [privileged: true]),
    containerTemplate(name: 'tools', image: 'alpine/helm:3.14.2', command: '/bin/sh', args: '-c cat', tty: true)
]) 

pipeline {
    agent { label "docker-build-agent" }

    environment {
        DOCKER_REGISTRY_USER = 'bassant41656' 
        APP_NAMESPACE = 'dev'
        HELM_RELEASE_NAME = '3tier-app-release'
    }

    stages {
        stage("Source Code") {
            steps {
                echo "Checking source code..."
                git branch: 'HelmCI/CD',
                    url: 'https://github.com/Bassantmohy/three-tier-app-project/'
            }
            post {
                success { echo "Source code checked successfully" }
                failure { echo "Couldn't check source code" }
            }
        }

        stage("Deploy with Helm") {
            steps {
                container('tools') {
                    echo "Deploying Helm chart..."
                    sh """
                    helm upgrade --install $HELM_RELEASE_NAME ./3-tier-app \
                        --namespace $APP_NAMESPACE \
                        --values ./3-tier-app/values.yaml
                    """
                }
            }
        }

        stage("Smoke Test") {
            steps {
                container('tools') {
                    echo "Running Smoke Test..."
                    sh """
                    curl -k -s -o /dev/null -w "%{http_code}" https://proxy-svc.${APP_NAMESPACE}.svc.cluster.local/health | grep 200
                    """
                    echo "Smoke Test Passed: /health responded 200 OK."
                }
            }
        }
    }

    post {
        success {
            echo "======== Pipeline executed successfully ========"
            mail to: 'bassant16.mohy@gmail.com',
                 subject: "Jenkins Pipeline Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Good news! The pipeline ${env.JOB_NAME} build #${env.BUILD_NUMBER} succeeded.\nCheck it here: ${env.BUILD_URL}"
        }
        failure {
            echo "======== Pipeline execution failed ========"
            mail to: 'bassant16.mohy@gmail.com',
                 subject: "Jenkins Pipeline Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Oops! The pipeline ${env.JOB_NAME} build #${env.BUILD_NUMBER} failed.\nCheck it here: ${env.BUILD_URL}"
        }
    }
}
