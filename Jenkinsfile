pipeline {
    agent any
	environment {
		dockercred=credentials('docker')
	}
    stages {
        stage('Git Checkout') {
            steps {
            git branch: 'main', credentialsId: 'gitcred', url: 'git@github.com:skYth1an/diplom_app.git'
            }
        }
        stage('Build and Push image') {
            steps {
            script {
                sh '''checktag=$(git tag --points-at ${GIT_COMMIT})
                    if [ -z "$checktag" ]
                    then
                        dockertag=latest
                    else
                        dockertag=$(git tag --points-at ${GIT_COMMIT})
                    fi
                    docker build -t skyth1an/app:$dockertag .
                    echo $dockercred_PSW | docker login -u $dockercred_USR --password-stdin
                    docker push skyth1an/app:$dockertag
                    '''
                   }
                }
            }
        stage('Clean') {
            steps {
                cleanWs()
                   }
        }
    }
}