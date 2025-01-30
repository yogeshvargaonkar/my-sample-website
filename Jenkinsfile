pipeline {
    agent any

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
                sh 'docker build -t my-sample-website .'
            }
        }
	}
}