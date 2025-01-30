pipeline {
    agent any
    environment{
        APP_VERSION = "1.0.$BUILD_ID"
        APP_NAME    = "my-sample-website"
        AWS_ACCOUNT_NUMBER = "${params.aws_account_number}."
        AWS_ECR_REPOSITORY = "${params.aws_account_number}.dkr.ecr.us-east-1.amazonaws.com/my-sample-website"
        AWS_DEFAULT_REGION    = 'us-east-1'
    }

	stages {
        stage('Build Project Docker Image') {
            agent{
                docker{
                    image 'my-aws-cli'
                    reuseNode true
                    args "-u root -v /var/run/docker.sock:/var/run/docker.sock --entrypoint=''"
                }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'my-aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        docker build -t $AWS_ECR_REPOSITORY/$APP_NAME:$APP_VERSION .
                        aws ecr get-login-password | docker login --username AWS --password-stdin $AWS_ECR_REPOSITORY
                        docker push $AWS_ECR_REPOSITORY/$APP_NAME:$APP_VERSION
                    '''
                }
            }
        }

        stage('Update ECS Task definition') {
            agent{
                docker{
                    image 'my-aws-cli'
                    reuseNode true
                    args "-u root -v /var/run/docker.sock:/var/run/docker.sock --entrypoint=''"
                }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'my-aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        sed -i "s/#APP_VERSION#/$APP_VERSION/g" ecs-task-definition.json
                        sed -i "s/#AWS_ACCOUNT_NUMBER#/$AWS_ACCOUNT_NUMBER/g" ecs-task-definition.json
                        aws ecs register-task-definition --cli-input-json file://ecs-task-definition.json
                    '''
                }
            }
        }
	}
}