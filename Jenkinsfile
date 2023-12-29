pipeline {
    agent any

    environment {
        INSTANCE_ID = 'i-04b3b479c5a9940f4'
    }

    stages {
        stage('Start EC2 Instance') {
            steps {
                    script {
                        bat """
                        set AWS_ACCESS_KEY_ID=AKIATWI5MRG7JUGMX7XX
                        set AWS_SECRET_ACCESS_KEY=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
                        aws ec2 start-instances --region ap-south-1 --instance-ids %INSTANCE_ID%
                        """
                    }
            }
        }
    }
}
