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
        stage('Build and deploy') {
            steps {
            script {
                sh '''checktag=$(git tag --points-at ${GIT_COMMIT})
                    if [ -z "$checktag" ]
                    then
                        dockertag=latest
                        docker build -t skyth1an/app:$dockertag .
                        echo $dockercred_PSW | docker login -u $dockercred_USR --password-stdin
                        docker push skyth1an/myapp:$dockertag
                    else
                        dockertag=$(git tag --points-at ${GIT_COMMIT})
                        docker build -t skyth1an/app:$dockertag .
                        echo $dockercred_PSW | docker login -u $dockercred_USR --password-stdin
                        docker push skyth1an/myapp:$dockertag
                        git clone https://github.com/skYth1an/diplom_kuber.git
                        sed -i 's/app: skyth1an\/myapp:.*/app: skyth1an\/myapp:$dockertag/' diplom_kuber/values.yaml
                        helm upgrade mon -n monitoring diplom_kuber/
                    fi
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