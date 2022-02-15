
pipeline{

	agent any

	
	stages {

		stage('Build') {

			steps {
			  sh 'ls'
				sh 'sudo docker build -t showmikb .'
				sh 'sudo docker tag showmikb:latest 056984472572.dkr.ecr.us-east-1.amazonaws.com/showmikb:latest'
               
			}
		}
        
        stage('login') {

            steps {
        
		           sh 'sudo aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 056984472572.dkr.ecr.us-east-1.amazonaws.com'

            }
        }
		stage('Push') {

			steps {
				sh 'sudo docker push 056984472572.dkr.ecr.us-east-1.amazonaws.com/showmikb:latest'
			}
		}

        stage('eks deploy') {

			steps {
			  sh 'aws eks update-kubeconfig --region us-west-1 --name demo'
        sh 'kubectl apply -f deploy.yaml'
        sh 'kubectl rollout restart deployment server-demo'
	sh 'kubectl apply -f service.yaml'
	sh 'kubectl get nodes -o wide'
        sh 'aws iam get-role --role-name "AWSServiceRoleForElasticLoadBalancing" || aws iam create-service-linked-role --aws-service-name "elasticloadbalancing.amazonaws.com"'
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
