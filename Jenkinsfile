pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        INSTANCE_ID = 'i-04b3b479c5a9940f4'
        AWS_CLI_PATH = '"C:\\Program Files\\Amazon\\AWSCLIV2\\aws.exe"'
 
    }

    stages {
        stage('Start EC2 Instance') {
            steps {
                withCredentials([string(credentialsId: 'syncwell-jenkins', variable: 'AWS_CREDENTIALS')]) {
                    script {
                        bat "\"C:\\Program Files\\Amazon\\AWSCLIV2\\aws.exe\" ec2 start-instances --region ${env.AWS_REGION} --instance-ids ${env.INSTANCE_ID}"
                    }
                }
            }
        }
    }
}
