#2.9 Working with Kubernetes
#2.9.1 Install Minikube using official tutorial https://kubernetes.io/ru/docs/tasks/tools/install-minikube/
	
	minikube start

#2.9.2 Create namespace:

	kubectl create namespace web-app

#2.9.3 Create deployments.yaml file:

	nano web-app-deployment.yaml
	
	![Alt text](/screenshot/deployments.png?raw=true "Deployments.yaml")
	
#2.9.4 Install NGINX Ingress Controller:

	minikude addons enable ingress
	
#2.9.5 Create ingress rule:

	nano ingress-rule.yaml
	
	![Alt text](/screenshot/ingress.png?raw=true "Ingress.yaml")
	
	kubectl config set-context --current --namespace=web-app
	
	kubectl apply -f web-app-deployment.yaml
	
	kubectl create -f ingress-rule.yaml
	
	kubectl get pods -A
	
	kubectl get svc
	
	kubectl get all
	
	![Alt text](/screenshot/result.png?raw=true "Result")
	