# tutorial-kubernets

Commands in kubernets
kubectl
		create deployment <NAME> --image=<IMAGE> [--dry-run] [options]

<NAME> = name of the api
<IMAGE> = name the image

exemple : kubectl create deployment ngnix-deployment --image=nginx 
          kubectl create deployment mongo-deployment --image=mongo 

verifi: kubectl get deployment
        kubectl get pod
		
		
debugging pods
	kubectl logs <POD-NAME>
	kubectl describe pod <POD-NAME>
	kubectl exec -it <POD-NAME> -- bin/bash
	
Configuration
	kubectl apply -f <NAME_FILE>.yaml  
	kubectl delete -f <NAME_FILE>.yaml 
	
Secret
warning: Secret must be created before the Deployment

	base64 for windows: [convert]::ToBase64String([Text.Encoding]::UTF8.GetBytes("password"))
	
	kubectl apply -f mongo-secret.yaml
	kubectl get secret
	
	secret can be referenced now in Deployment
	
	kubectl apply -f mongodb-deployment.yaml
	
Service 
	kubectl apply -f <NAME_FILE>.yaml 
	kubectl get service
	kubectl get all | grep <LABEL_NAME_DEPLOYMENT>
	kubectl get all | grep mongodb-deployment
	grep : O termo 'grep' não é reconhecido como nome de cmdlet, função, arquivo de script ou programa operável NO WINDOWS
	kubectl get all | *mongodb*

Deployment Service and Config MAP
	Exemple : mongo-express
	view: https://hub.docker.com/_/mongo-express
		How to use this image
			port : 8081

		which database to connect?
			ME_CONFIG_MONGODB_SERVER        | 'mongo'         | MongoDB container name. Use comma delimited list of host names for replica sets.
			
		which credentials to authenticate?
			ME_CONFIG_MONGODB_ADMINUSERNAME | ''              | MongoDB admin username
			ME_CONFIG_MONGODB_ADMINPASSWORD | ''              | MongoDB admin password
	Config MAP
		-external configuration
		-centralized
		-other components can use it
		
		database_url: <SERVICE_NAME>
		
		ConfigMap must already be in the k8s cluster, when referencing it
		
	warning:
		create ConfigMap-deployment first
		kubectl apply -f mongo-configmap.yaml
		kubectl apply -f mongo-express-deployment.yaml
		
Deployment Mongo Express External Service

	How to make it an External Service?

	type: LoadBalancer

		this assigns service an external IP address and so accepts external request
		
	nodePort: 

		this port for external IP addres
		port you need to put into browser
		must de between 30000-32000
		
		Exemple
	kubectl get service
	NAME                    TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE 
	kubernetes              ClusterIP      10.96.0.1       <none>        443/TCP          100d
	mongo-express-service   LoadBalancer   10.101.98.80    <pending>     8081:30000/TCP   31s 
	mongodb-service         ClusterIP      10.108.30.246   <none>        27017/TCP        83m 
	
	minikube service mongo-express-service
	
all



kubectl apply -f <NAME_FILE>.yaml 

kubectl apply -f mongo-configmap.yaml
kubectl apply -f mongo-express-deployment.yaml
kubectl apply -f mongo-secret.yaml
kubectl apply -f mongodb-deployment.yaml

