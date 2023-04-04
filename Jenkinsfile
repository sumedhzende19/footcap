pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // Build your application and create a Docker image
                sh 'docker build -t footcap .'
            }
        }
        stage('Push') {
            steps {
                // Push the Docker image to your ECR repository
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: "aws key", secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                    sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/n2t9e0y8'
                    sh 'docker tag footcap public.ecr.aws/n2t9e0y8/footcap:latest'
                    sh 'docker push public.ecr.aws/n2t9e0y8/footcap:latest'
                }
            }
        }
        // stage('Deploy') {
        //     steps {
        //         // Deploy the Docker image to your ECS cluster and service
        //         ecsUpdateService(
        //             cluster: 'footcap',
        //             service: 'footcap-test-service',
        //             image: 'public.ecr.aws/n7a1b8r0/footcap-nginx:latest',
        //             region: 'us-east-2',
        //             waitForServiceCapacity: true,
        //             taskDefinitionOverride: ''
        //         )
        //     }
        // }
    }
}
