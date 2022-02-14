
pipeline{

	agent any

	
	stages {

		stage('Build') {

			steps {
				sh 'sudo docker build -t showmikb .'
				sh 'sudo docker tag showmikb:latest 056984472572.dkr.ecr.us-east-1.amazonaws.com/showmikb:latest'
               
			}
		}
        
        stage('login') {

            steps {
        
		           sh 'sudo aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 056984472572.dkr.ecr.us-east-1.amazonaws.com'

            }
        }
		stage('Push') {

			steps {
				sh 'sudo docker push 056984472572.dkr.ecr.us-east-1.amazonaws.com/showmikb:latest'
			}
		}

        stage('eks deploy') {

			steps {
        sh 'sudo kubectl apply -f deploy.yaml'
        sh 'sudo kubectl rollout restart deployment server-demo'
			}
		}
	}

	post {
		always {

            		cleanWs()
			
            		echo "done"
		}
	}

}
