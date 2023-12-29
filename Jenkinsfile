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
                    aws ec2 start-instances --region $AWS_REGION --instance-ids $INSTANCE_ID
                }
            }
        }
    }
}
