# KUBE

## Kubernetes Demo



## Building and Publishing

**Note:** if you build your own site update index.html with your site content.


<h1 align="center" style="border-bottom: none;">🔎 Kubernetes 101 </h1>
<h3 align="center">Kubernetes is an Open-source container orchestration system for automating software deployment, scaling, and management</h3>


### Flow

In this topic, you'll follow a series of hands-on exercises that demonstrate how to use Kubernetes for your applications. You'll start with the basics: creating and running your first Kubernetes cluster, pod and service. By the end of the course, you'll get a brief introduction to running Kubernetes in production.



Enjoy this topic!

<h3>APPM-5350</h3>
</p>

1. login to the development virtual machine VM hosted on AWS cloud

	 `
    ssh ubuntu@XX.XXX.XXX.XXX
    `
    
    *Make sure to post your **ssh public key** to the Slack channel used for this lesson
    
2. Navigate to class subdirectory

	`cd /home/ubuntu/5720`

4. Create & navigate to your own directory

	`mkdir userName`
	
	`cd userName`
	
	For example:
	
	`mkdir ivanp`
	
	`cd ivanp`
	
	
5. Clone the **kube** repository from github

	`git clone https://github.com/iportilla/kube.git`
	
6. Change directory to the **kube** directory

	`cd kube/`
7. Install `minikube` https://www.linuxtechi.com/how-to-install-minikube-on-ubuntu/ & Test your `kube` installation by running the following command:

	`minikube status`
	
	You will see:
	
	```
	minikube
	type: Control Plane
	host: Running
	kubelet: Running
	apiserver: Running
	kubeconfig: Configured
	
	```
	
### Create a `Deployment`

7. Next, we will create a Kubernetes Deployment and Pod using a public image hosted on a container repository (Google.io).
   
   See [https://kubernetes.io/docs/tutorials/hello-minikube/](https://kubernetes.io/docs/tutorials/hello-minikube/) for details

A Kubernetes `Pod` is a group of one or more Containers, tied together for the purposes of administration and networking. The Pod in this tutorial has only one Container. A Kubernetes `Deployment` checks on the health of your Pod and restarts the Pod's Container if it terminates. Deployments are the recommended way to manage the creation and scaling of Pods.

Use the `kubectl create` command to create a **Deployment** that manages a Pod. The Pod runs a Container based on the provided Docker image that includes a web server:

`kubectl create deployment hello-node-name --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080`

You will see:

`deployment.apps/hello-node created`

The `kubectl create deployment` command fetches the hello image from the Docker registry and saves it to your system. You can use the `kubectl get services` to see a list of all services running on your system.
	
`kubectl get services`


You will see something like:

```
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   21m
 ```

9. Great! Let's see the Pods running:

 `kubectl get pods`

You will see:

```
NAME                                  READY   STATUS    RESTARTS        AGE
hello-node-744769d864-jlphj           1/1     Running   0               82m
```

10. Deploy your first app:

    [https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/](https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/)


    Run:
    `kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1`

The API server will automatically create an endpoint for each pod, based on the pod name, that is also accessible through the proxy.

First we need to get the Pod name, and we'll store in the environment variable POD_NAME:

`export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME`

You can access the Pod through the proxied API, by running:

`curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/`

## Deploy your own app

1. Build the app with your name as $DOCKER_ID:app:v1.0.0
   
   `docker build -t iportilla/app:1.0.0 -f ./Dockerfile.amd64  .`

2. Push your app to the repository
   
   `docker push iportilla/app:1.0.0`

3. Create the pod and service objects in K8S with:

```
kubectl apply -f pod.yaml
kubectl apply -f service.yaml
```

4. Validate application works:

`kubectl get service`

5. Let's verify that the application we deployed in the previous scenario is running. We'll use the kubectl get command and look for existing Pods:

`kubectl get pods`

Next, to view what containers are inside that Pod and what images are used to build those containers we run the kubectl describe pods command:

`kubectl describe pods`

6.  Next, we'll get the Pod name and query that pod directly through the proxy. To get the Pod name and store it in the POD_NAME environment variable

`export POD_NAME="$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')"
echo Name of the Pod: $POD_NAME`

To see the output of our application, run a curl request:

`curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME:8080/proxy/`


## Resources:

https://minikube.sigs.k8s.io/docs/handbook/

https://www.linuxtechi.com/how-to-install-minikube-on-ubuntu/

https://github.com/iportilla/kubernetes-101-workshop

    
## License

This sample code is licensed under the [MIT License](https://opensource.org/licenses/MIT).

## Open Source @ IBM

Find more open source projects on the [IBM Github Page](http://ibm.github.io/)

