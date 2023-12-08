# KUBE

## Kubernetes Demo



## Building and Publishing

**Note:** if you build your own site update index.html with your site content.


<h1 align="center" style="border-bottom: none;">🔎 Kubernetes 101 </h1>
<h3 align="center">Kubernetes is an Open-source container orchestration system for automating software deployment, scaling, and management</h3>


### Flow

In this topic, you'll follow a series of hands-on exercises that demonstrate how to use Kubernetes for your applications. You'll start with the basics: creating and running your first Kubernetes cluster, pod and service. By the end of the course, you'll get a brief introduction to running Kubernetes in production.



Enjoy this topic!

<h3>APPM-5720</h3>
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
7. Test your `kube` installation by running the following command:

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

Use the `kubectl create` command to create a **Deployment** that manages a Pod. The Pod runs a Container based on the provided Docker image.
   

	```
	# Run a test container image that includes a webserver
	kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080
	```
	You will see:
	
	```	
	deployment.apps/hello-node created
	```

	The `kubectl create deployment` command fetches the hello image from the Docker registry and saves it to your system. You can use the `kubectl get services` to see a list of all services running on your system.
	
	`kubectl get services`

 	You will see something like:

  	```
   	NAME                  TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)          AGE
	kubernetes            ClusterIP      10.96.0.1       <none>         443/TCP          20h
	kubernetes-bootcamp   NodePort       10.97.168.128   <none>         8080:30345/TCP   116m
	simpleapp             LoadBalancer   10.100.171.6    10.100.171.6   80:31458/TCP     160m
   	```

9. Great! Let's now run a Docker container based on this image. Run the following command:

 `docker run busybox echo "Hello World from busybox`

9. Let's run a terminal in the busybox container with:

 `docker run -it busybox /bin/sh`
 
 Test you are running inside the container with:
 
 `ls`
 
 You will see:
 
 ```
bin   dev   etc   home  proc  root  sys   tmp   usr   var
 ```
 
 Exit the container
 
 `exit`

### Static Site

The first thing we're going to look at is how we can run a dead-simple static website. We're going to pull a Docker image from Docker Hub, run the container and see how easy it is to run a webserver.

1. Detached mode, run

`docker run -d -p 80:80 --name static-site prakhar1989/static-site`

In the above command, `-d` will detach our terminal, `-P` will publish all exposed ports to `80:80` and finally `--name` corresponds to a name we want to give. Now we can see the ports by running the `docker port [CONTAINER]` command:

`docker port static-site`

Check your browser to

`http://XX.XXX.XXX.XXX`

Using the IP address above.

Check running containers with

`docker ps`

Stop running container with:

`docker stop static-site`

Prune stopped containers with:

`docker container prune`


## Docker Images

We have looked at images before, but in this section we'll dive deeper into what Docker images are and build our own image! Lastly, we'll also use that image to run our application locally and finally deploy on AWS to share it with our friends! Excited? Great! Let's get started.

Docker images are the basis of containers. In the previous example, we pulled the Busybox image from the registry and asked the Docker client to run a container based on that image. To see the list of images that are available locally, use the docker images command.

`docker images`

Let's create an webapp image with the followig `Make` commands:


1. To reset Docker images:

   ```
   make clean
   ```

1. Build the image, type:

   ```
   make build
   ```

1. Run the container, type:

   ```
   make run
   ```

4. To stop the docker container, type:

   ```
   make stop
   ```

1. View the application in a browser at `XX.XXX.XXX.XXX:PORT`

where `XX.XXX.XXX.XXX` is the IP we used in the login step above and `PORT` is the port number provided in the `.env` file earlier.


## License

This sample code is licensed under the [MIT License](https://opensource.org/licenses/MIT).

## Open Source @ IBM

Find more open source projects on the [IBM Github Page](http://ibm.github.io/)

