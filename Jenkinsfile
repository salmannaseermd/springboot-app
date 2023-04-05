pipeline {
    agent any

    environment {
        registry = "373595631462.dkr.ecr.us-east-1.amazonaws.com/springboot_ecr"
    }
    
     stages {
    //     stage('Checkout') {
    //         steps {
    //             checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/akannan1087/springboot-app']])
    //         }
    //     }
        
        stage ("build Jar") {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage ("Build image") {
            steps {
                script {
                    docker.build registry
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
            steps {
                sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 373595631462.dkr.ecr.us-east-1.amazonaws.com"
                sh "docker push 373595631462.dkr.ecr.us-east-1.amazonaws.com/springboot_ecr:latest"
            }
        
        // stage ("Deploy to K8S") {
        //     steps {
        //         withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8S', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
        //         sh "kubectl apply -f eks-deploy-k8s.yaml"
                    
        //         }
        //     }
        // }
                    }
            }
         
     }
}