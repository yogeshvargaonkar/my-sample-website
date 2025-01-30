pipeline {
    agent any

	stages {
        stage('Build Project Docker Image') {
            
            steps {
                sh 'docker build -t my-sample-website .'
            }
        }
	}
}