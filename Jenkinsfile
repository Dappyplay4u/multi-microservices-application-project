def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
    'UNSTABLE': 'danger'
]

pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-west-2'
    }

    stages {
        // Checkout To The Microservice Branch
        stage('Checkout To Microservice Branch') {
            steps {
                git branch: 'app-database', url: 'https://github.com/Dappyplay4u/multi-microservices-application-projects.git'
            }
        }

        // Configure AWS CLI before deployment
        stage('Configure AWS CLI') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'aws-credentials', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                        sh '''
                            aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                            aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                            aws configure set region $AWS_DEFAULT_REGION
                        '''
                    }
                }
            }
        }

        // Deploy to The Staging/Test Environment
        stage('Deploy Microservice To The Stage/Test Env') {
            steps {
                script {
                    withKubeConfig(
                        caCertificate: '',
                        clusterName: '',
                        contextName: '',
                        credentialsId: 'Kubernetes-Credential',
                        namespace: '',
                        restrictKubeConfigAccess: false,
                        serverUrl: ''
                    ) {
                        sh 'kubectl apply -f deploy-envs/test-env/deployment.yaml'
                        sh 'kubectl apply -f deploy-envs/test-env/service.yaml'  // ClusterIP Service
                    }
                }
            }
        }

        // Production Deployment Approval
        stage('Approve Prod Deployment') {
            steps {
                input('Do you want to proceed?')
            }
        }

        // Deploy to The Production Environment
        stage('Deploy Microservice To The Prod Env') {
            steps {
                script {
                    withKubeConfig(
                        caCertificate: '',
                        clusterName: '',
                        contextName: '',
                        credentialsId: 'Kubernetes-Credential',
                        namespace: '',
                        restrictKubeConfigAccess: false,
                        serverUrl: ''
                    ) {
                        sh 'kubectl apply -f deploy-envs/prod-env/deployment.yaml'
                        sh 'kubectl apply -f deploy-envs/prod-env/service.yaml'  // ClusterIP Service
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Slack Notifications.'
            slackSend channel: '#all-minecraftapp',
            color: COLOR_MAP[currentBuild.currentResult],
            message: "*${currentBuild.currentResult}:* Job Name '${env.JOB_NAME}' build ${env.BUILD_NUMBER} \nBuild Timestamp: ${env.BUILD_TIMESTAMP} \nProject Workspace: ${env.WORKSPACE} \nMore info at: ${env.BUILD_URL}"
        }
    }
}
