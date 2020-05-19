pipeline {
    agent any
    stages {
        stage('Lint') {
            steps {
                sh 'tidy -q -e *.html'
            }
        }
        stage('Docker Build') {
            steps {
                sh "docker build -t udacity-capstone:latest ."
            }
        }
        stage('ECR login') {
            steps {
                withAWS(credentials:'udacity-capstone') {
                    script {
                        def login = ecrLogin()
                        sh "${login}"
                    }
                }
            }
        }
        stage ('Docker Push') {
            steps {
                sh 'docker tag udacity-capstone:latest 120106008631.dkr.ecr.us-east-1.amazonaws.com/udacity-capstone:latest'
                sh 'docker push 120106008631.dkr.ecr.us-east-1.amazonaws.com/udacity-capstone:latest'
            }
        }
        stage('EKS Cluster') {
            steps {
                sh 'eksctl create cluster --name capstone-cluster --region us-east-1 --zones=us-east-1a,us-east-1b,us-east-1c --node-type t2.small --nodes 2 --managed'
            }
        }
    }
}