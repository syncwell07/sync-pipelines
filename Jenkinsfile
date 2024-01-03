import groovy.json.JsonSlurper
pipeline {
    agent any

    environment {
        INSTANCE_ID = 'i-002953cac36e74bd0'
        AWS_REGION = 'ap-south-1'
        EC2_USERNAME = 'ubuntu'  // Replace with your EC2 instance's username
        PPK_CREDENTIALS_ID = 'risk-ppk-file'
        SSH_CREDENTIALS_ID = 'sync-ssh'
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
                            bat(script: "aws ec2 describe-instances --region ${env.AWS_REGION} --instance-ids ${env.INSTANCE_ID} --output json > output.json")
                            def output = readFile('output.json').trim()
                            def jsonSlurper = new JsonSlurper()
                            def jsonObject = jsonSlurper.parseText(output)
                            env.EC2_PUBLIC_DNS = jsonObject.Reservations[0].Instances[0].PublicDnsName
                            echo "EC2 Instance Public DNS: ${EC2_PUBLIC_DNS}"
                        } else {
                            error "Failed to start EC2 instance"
                        }
                    }
                }
            }
        }
        
        stage('clone repo') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY',
                    credentialsId: 'risk-aws'
                ]]) {
                    script {
                        echo '{"workingDirectory": [""],"executionTimeout": ["3600"],"commands": ["git clone https://github.com/sanjayambatkar/risklab-ui.git"]}' > parameters.json
                        bat script: 'aws ssm send-command --document-name "AWS-RunShellScript" --document-version "1" --targets "[{\"Key\":\"InstanceIds\",\"Values\":[\"i-002953cac36e74bd0\"]}]" --parameters "commands": ["git clone https://github.com/sanjayambatkar/risklab-ui.git"] --timeout-seconds 600 --max-concurrency "50" --max-errors "0" --region ap-south-1', returnStatus: true
                    }
                }
            }
        }

    }
}
