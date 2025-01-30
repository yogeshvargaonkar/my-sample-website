pipeline {
    
    environment{
        APP_VERSION = "1.0.$BUILD_ID"
        APP_NAME    = "my-sample-website"

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
                sh 'docker build -t $APP_NAME:$APP_VERSION .'
            }
        }
	}
}