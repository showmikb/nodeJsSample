
pipeline{

	agent any

	
	stages {

		stage('Build') {

			steps {
				sh 'docker build -t showmikb .'
				sh 'docker tag showmikb:latest 056984472572.dkr.ecr.us-east-1.amazonaws.com/showmikb:latest'
               
			}
		}
        
        stage('login') {

            steps {
        
		           sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 056984472572.dkr.ecr.us-east-1.amazonaws.com'

            }
        }
		stage('Push') {

			steps {
				sh 'docker push 056984472572.dkr.ecr.us-east-1.amazonaws.com/showmikb:latest'
			}
		}

        stage('eks deploy') {

			steps {
				sh 'kubectl get -o yaml deploy/hello-world-nodejs > deploy.yaml'
        sh "sed -i 's/hellonodejs:latest/hellonodejs:eks/g' deploy.yaml"
        sh 'kubectl apply -f deploy.yaml'
        sh 'kubectl rollout restart deployment hello-world-nodejs'
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
