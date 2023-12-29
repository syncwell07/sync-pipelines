pipeline {
    agent any

    stages {
        stage('Run CMD Command') {
            steps {
                script {
                    // Replace 'your-cmd-command' with your actual CMD command
                    def cmdCommand = 'aws ec2 start-instances --region ap-south-1 --instance-ids i-04b3b479c5a9940f4'

                    // Run the CMD command using the 'bat' step
                    bat cmdCommand
                }
            }
        }
    }
}
