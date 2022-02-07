ACCOUNT = '873382687726'
AWS_REGION = 'eu-west-1'
REPO_NAME = 'iib-docker'
IMAGE_NAME = 'latest'
DOCKER_DEST_REGISTRY = "${ACCOUNT}.dkr.ecr.${AWS_REGION}.amazonaws.com/$REPO_NAME"
DEST_IMAGE = "${DOCKER_DEST_REGISTRY}:${IMAGE_NAME}"

pipeline{
		agent {label 'docker-builder'}
		stages {

			stage('Code Checkout') {
				steps {
                    script {
                        checkout scm
                    }
				}
			}

			stage('Build Image') {
				steps {
                    withAWS(role:'external-jenkins-access', roleAccount:"$ACCOUNT", region:"$AWS_REGION") {
                        script {
                            def login = ecrLogin()
                            sh login
                            sh "docker build -t $DEST_IMAGE ."
                        }
                    }
				}
			}

			stage('Push Image') {
				steps {
                    withAWS(role:'external-jenkins-access', roleAccount:"$ACCOUNT", region:"$AWS_REGION") {
                        script {
                            def login = ecrLogin()
                            sh login
                            docker.image("${DEST_IMAGE}").push()

                        }
                    }

				}
			}
		}
}
