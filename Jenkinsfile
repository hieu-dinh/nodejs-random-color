pipeline {
    agent any
    stages {
        // stage('Checkout') {
        //     steps {
        //         git 'https://github.com/hoanglinhdigital/nodejs-random-color.git'
        //     }
        // }

        stage('Build') {
            steps {
                sh 'docker build -t nodejs-random-color:ver-${BUILD_ID} .'
            }
        }
        stage('Upload image to ECR') {
            steps {
                // Upload the artifact to ECR with AWS credential
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', 
                                  credentialsId: 'jenkins_aws_credential', 
                                  accessKeyVariable: 'AWS_ACCESS_KEY_ID', 
                                  secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']])
                                  {
                                      sh '''
                                        aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 613122529343.dkr.ecr.ap-southeast-1.amazonaws.com
                                        docker tag nodejs-random-color:ver-${BUILD_ID} 613122529343.dkr.ecr.ap-southeast-1.amazonaws.com/nodejs-random-color:ver-${BUILD_ID}
                                        docker push 613122529343.dkr.ecr.ap-southeast-1.amazonaws.com/nodejs-random-color:ver-${BUILD_ID}
                                      '''
                                  }
            }
        }
    }
}
