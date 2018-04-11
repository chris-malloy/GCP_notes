# Google Container Engine

## Lets you run Container Images like Docker and Kubernetes

### Setup Needed for Deployment

* enable Kubernetes API
* configure zone
* spin up a container cluster with desired nodes and permissions
* change your-project-id to $DEVSHELL_PROJECT_ID in config and yaml files
* configure your app as the default container cluster
* build a docker image and push it to container registry
* push built image to container registry
* configure kubectl command utility with credentials
* deploy app using kubectl command with yaml file

### Shell Commands

View a list of available zones in which to create your container cluster

```shell
gcloud compute zones list
```

Configure Cloud SDK with a default zone where <ZONE> is your chosen zone

```shell
gcloud config set compute/zone <ZONE>
```

Review your current Cloud SDK configuration

```shell
gcloud config list
```

Spin up a container cluster.  Scopes are needed for permissions

```shell
gcloud container clusters create bookshelf \
  --scopes <SCOPES> \
  --num-nodes <NUM_NODES>
```

Replace default app name in config file and yaml file with correct app name

```shell
sed -i s/your-project-id/$DEVSHELL_PROJECT_ID/ config.py
```

Configure your app as the default container cluster for Cloud SDK

```shell
gcloud config set container/cluster bookshelf
```

Build docker image and tag it with the URL for the container registry in your project

```shell
docker build -t gcr.io/$DEVSHELL_PROJECT_ID/bookshelf .
```

Push built docker image to container registry using Cloud SDK

```shell
gcloud docker -- push gcr.io/$DEVSHELL_PROJECT_ID/bookshelf
```

Retreive credentials for kubectl command utility

```shell
gcloud container clusters get-credentials bookshelf
```

Deploy app using kubectl command with yaml file

```shell
kubectl create -f bookshelf-frontend.yaml
```

Retreive the external IP of the load balancer

```shell
kubectl get services bookshelf-frontend
```

### Kubernetes Terms

Nodes - A node is a worker machine in a Kubernetes cluster, and in Google Kubernetes Engine, the machine is always a Compute Engine instance. In this lab, you create a cluster with two Compute Engine nodes.

Pods - A pod is a group of one or more containers, shared storage, and configuration data relating to those containers. It is common for production applications running in Kubernetes to include multiple, relatively tightly-coupled containers in a single pod. For the purpose of this simple demonstration however, a single Bookshelf Docker container runs inside each pod.

Replication Controllers - A replication controller works to ensure that the requested number of pod replicas are always available and running at a given time. The replication controller automatically adds or removes pods as required to maintain a desired state. In the context of this lab, you specify that the replication controller should create three replicas, running across your two nodes. Once you deploy the replication controller, there will be three identical pods running the frontend component of Bookshelf.

Services - A service defines a logical set of pods and a way to access them using an IP address and port number pair. In the context of this lab, the Bookshelf application is exposed to end-users as a service using a Google Cloud Platform network load-balancer, and external IP address on port 80.

### Other Relevant Topics

* [YAML](https://en.wikipedia.org/wiki/YAML)
