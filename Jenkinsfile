pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        INSTANCE_ID = 'i-04b3b479c5a9940f4'
    }

    stages {
        stage('Start EC2 Instance') {
            steps {
                script {
                    // Use the sh step to run shell commands
                    bat "aws ec2 start-instances --region ${env.AWS_REGION} --instance-ids ${env.INSTANCE_ID}"
                }
            }
        }
    }
}
