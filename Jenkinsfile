import groovy.json.JsonSlurper
pipeline {
    agent any

    environment {
        INSTANCE_ID = 'i-057465c802925c0ef'
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

        stage('Run Commands on EC2 Instance') {
            steps {
                script {
                    // Use the 'withCredentials' step to run commands on the EC2 instance
                    sshagent(credentials: [env.SSH_CREDENTIALS_ID]) {
                        // Now you can use ssh commands securely
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@${env.EC2_PUBLIC_DNS} 'sudo docker ps'"
                    }
                }
            }
        }
    }
}
