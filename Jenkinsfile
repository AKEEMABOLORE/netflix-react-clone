pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("343137909755.dkr.ecr.eu-west-2.amazonaws.com/akeem3.netflixapp:latest")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 343137909755.dkr.ecr.eu-west-2.amazonaws.com/akeem3.netflixapp:latest'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://343137909755.dkr.ecr.eu-west-2.amazonaws.com/akeem3.netflixapp', 'ecr:eu-west-2:akeemaws') {
                    // build image
                    def myImage = docker.build("343137909755.dkr.ecr.eu-west-2.amazonaws.com/akeem3.netflixapp:latest")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
