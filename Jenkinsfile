pipeline {
    agent any

    environment {
        AWS_REGION = 'your-aws-region'
        INSTANCE_ID = 'your-instance-id'
    }

    stages {
        stage('Start EC2 Instance') {
            steps {
                script {
                    aws ec2 start-instances --region $AWS_REGION --instance-ids $INSTANCE_ID
                }
            }
        }
    }
}
