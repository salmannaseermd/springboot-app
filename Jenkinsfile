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
// if you're getting this error => ERROR: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied
// then vim /etc/group and add the following line docker:x:123:jenkins
// the above line is to add jenkins user to docker group
        stage ("Build image") {
            steps {
                script {
                    docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
// To push to ECR follow these steps
// 1. Install AWS CLI in jenkins server
// 2. aws configure in jenkins USER
// 3. Then go to Jenkins gui and install the docker and docker pipeline plugin
// 4. Below 2 commands are available in aws console only in ecr webpage in "view push commands"
//    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 373595631462.dkr.ecr.us-east-1.amazonaws.com"
//    sh "docker push 373595631462.dkr.ecr.us-east-1.amazonaws.com/springboot_ecr:latest" 
        
        
        stage ("Push to ECR") {

            steps {
                sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 373595631462.dkr.ecr.us-east-1.amazonaws.com"
                // sh "docker push 373595631462.dkr.ecr.us-east-1.amazonaws.com/springboot_ecr:latest"
                sh "docker push 373595631462.dkr.ecr.us-east-1.amazonaws.com/springboot_ecr:$BUILD_NUMBER"
                    
            }
        }
            

    //     stage('Remove Unused docker image - Master') {
    //         steps{
    //     sh "docker rmi $imagename:$BUILD_NUMBER"
    //      sh "docker rmi $imagename:latest"

    //   }
      
        // stage('remove old docker images from jenkins server'){
        //     steps{
                
        //         sh "docker rmi $(docker images --filter dangling=true -q)"
        //     }
        // }

                stage('Remove Unused docker image') {
          steps{
            sh "docker rmi $registry:$BUILD_NUMBER"
          }
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