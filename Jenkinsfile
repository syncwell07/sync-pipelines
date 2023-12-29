pipeline {
    agent any

    environment {
        INSTANCE_ID = 'i-04b3b479c5a9940f4'
    }

    stages {
        stage('Start EC2 Instance') {
            steps {
               withCredentials([awsCredentials(credentialsId: 'risk-aws', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY', regionVariable: 'AWS_DEFAULT_REGION')]) {
                    script {
                        bat """
			echo %AWS_CREDENTIALS%
                        set AWS_ACCESS_KEY_ID=!AWS_CREDENTIALS.split(':')[0]!
                        set AWS_SECRET_ACCESS_KEY=!AWS_CREDENTIALS.split(':')[1]!
                        aws ec2 start-instances --region ap-south-1 --instance-ids %INSTANCE_ID%
                        """
                    }
                }
            }
        }
	}
}
