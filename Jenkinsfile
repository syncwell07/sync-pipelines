pipeline {
    agent any

    environment {
        INSTANCE_ID = 'i-04b3b479c5a9940f4'
	    AWS_REGION = 'ap-south-1'
    }

    stages {
        stage('Start EC2 Instance') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY',
                    credentialsId: 'risk-aws'
                ]]) {
                    script {
                        def ec2Info = bat script: "aws ec2 start-instances --region ${env.AWS_REGION} --instance-ids ${env.INSTANCE_ID} --query 'Instances[0].PublicDnsName' --output text", returnStatus: true
                        if (ec2Info == 0) {
                            // Set the public DNS as an environment variable for use in the next stage
                            env.EC2_PUBLIC_DNS = bat(script: "aws ec2 describe-instances --region ${env.AWS_REGION} --instance-ids ${env.INSTANCE_ID} --query 'Reservations[0].Instances[0].PublicDnsName' --output text", returnStdout: true).trim()
                            echo %EC2_PUBLIC_DNS%
			            } else {
                            error "Failed to start EC2 instance"
                        }
                    }
                }
            }
        }
	}
}
