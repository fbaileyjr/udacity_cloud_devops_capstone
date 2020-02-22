pipeline {
    environment {
    registry = "spiralmemory/udacity-devops-capstone"
    registryCredential = 'docker-hub'
    }
    
    agent any
    stages {
        
        stage ('Performing Linting on the HTML') {
            steps {
                sh 'tidy -q -e *.html'
            }
        }

        stage ('Cloning from udacity_cloud_devops_capstone') {
            steps {
                git 'https://github.com/fbaileyjr/udacity_cloud_devops_capstone.git'
            }
        }

        stage('Building the Docker Image') {
            steps {
                script {
                    sh 'docker build --tag=spiralmemory/udacity-devops-capstone .'
                }
            }
        }

        stage('Deploying the Image to Docker') {
            steps {
                script {
                    withDockerRegistry([ credentialsId: "docker-hub", url: "" ]) {
                    sh 'docker push spiralmemory/udacity-devops-capstone'
                    }
                }
            }
        }

        stage ('Upload latest green deployment to AWS Loadbalancer') {
            steps {
               script {
                    withAWS(credentials: 'aws-credentials', region: 'us-east-2'){
                   
                   sh "aws eks update-kubeconfig --region us-east-2 --name udacitycluster"
                   sh 'kubectl apply -f zone_configurations/green-zone.yml'
                  }
               }
            }
        }

        stage ('Remove old blue deployment from AWS Loadbalancer') {
            steps {
               script {
                withAWS(credentials: 'aws-credentials', region: 'us-east-2'){
                   sh 'kubectl delete zone_configurations/blue-deployment'
               }
               }
            }
        }

        stage ('Add latest blue deployment to AWS Loadbalancer') {
            steps {
               script {
                   withAWS(credentials: 'aws-credentials', region: 'us-east-2'){
                   sh 'kubectl apply -f zone_configurations/blue-zone.yml'
                   }
               }
            }
        }

        stage ('Remove old green deployment from AWS Loadbalancer') {
            steps {
               script {
                   withAWS(credentials: 'aws-credentials', region: 'us-east-2'){
                   sh 'kubectl delete zone_configurations/green-deployment'
                   }
               }
            }
        }
    }
}