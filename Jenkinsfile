pipeline {
    agent any 
        environment {
            GIT_COMMIT = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
        }
        stages {
        stage('Git Clone') {
            steps {
                git 'https://github.com/ShariqueAhmedGithub/ExpressJSHelloWorld.git'
            }
        }
        stage('Build Image') { 
            steps {
                script{
                    sh 'docker build -t 966873824982.dkr.ecr.ap-south-1.amazonaws.com/pearlthoughts:$GIT_COMMIT .'
                }
            }
        }
        stage('Push Image') { 
            steps {
                script {
                sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 966873824982.dkr.ecr.ap-south-1.amazonaws.com'
                sh 'docker push 966873824982.dkr.ecr.ap-south-1.amazonaws.com/pearlthoughts:$GIT_COMMIT'
            }
        }
        }
        stage('CF Stack') { 
            steps {
                script {
                sh 'aws cloudformation deploy --stack-name PearlThoughts --template-file /var/lib/jenkins/workspace/PearlThoughtsAssessment/CFTForECS.yaml --capabilities CAPABILITY_IAM --parameter-overrides ImageUrl=966873824982.dkr.ecr.ap-south-1.amazonaws.com/pearlthoughts:$GIT_COMMIT'
            }
        }
        }
    }
}