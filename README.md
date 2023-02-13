# About the project

### Kubernetes API [docs](https://kubernetes.io/docs/reference/kubernetes-api/)

This project is just a quick example of how kubernetes can work with the declarative files and also how to access other deploy by using only the APP name as the dns. The project contains a simple node.js application with express for testing purposes only.

# Installation

### Prerequisites

- Node.js v14.18.1 [install here](https://nodejs.org/ja/blog/release/v14.18.1/)
- Docker desktop [install here](https://www.docker.com/products/docker-desktop/)
- Minikube [install here](https://minikube.sigs.k8s.io/docs/start/)
- Kubectl [install here](https://kubernetes.io/docs/tasks/tools/)

### Cloning the repo

Go to the folder where you want to clone the repo and:

```bash
git clone https://github.com/joao-bermal/k8s
```

### Creating a cluster with minikube

You must use one of the driver options that you can find in [this page](https://minikube.sigs.k8s.io/docs/drivers/). For this tutorial I used docker to an easier setup since we installed docker-desktop already.

```bash
minikube start --driver=<driver-option>
```

### Running the project locally

In order to run the project, you can just apply both .yaml files using the kubectl like:

```bash
kubectl apply -f k8s-web-to-nginx.yaml -f nginx.yaml
```

In the .yaml files, we've created 5 instances (defined in the replica section) of each pod template (defined in the template section), also, in both files we've created 2 sections, the deployment and the service.

### kubectl commands

[Here](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) you can find a cheat sheet of commands that you can execute with kubectl. I'll leave some of them bellow, they can be used to check what you've just created:

```bash
kubectl get pods
```

```bash
kubectl get deploy
```

```bash
kubectl get svc
```

### Using minikube to get the app url

In order to see what is running you can use this command:

```bash
minikube service k8s-web-to-nginx
```

This will open a tab in your browser with the node application requesting in the root endpoint ("/"), to see the magic happening, access the ("/nginx") endpoint.

With this, we are accessing the nginx only by calling the name defined in the nginx.yaml file "http://nginx" as it is called in the index.mjs code as bellow:

```
app.get("/nginx", async (req, res) => {
  const url = "http://nginx";
  const response = await fetch(url);
  const body = await response.text();
  res.send(body);
});
```

The nginx service is a clusterIP which means that it is only accessible inside the cluster. If you try to reach the nginx service directly it won't work but in the application we are inside the cluster so it is accessible.

### Deleting the pods, deploys and services that we've created

In order to delete all the stuff we've created, you can run one of the following commands:

```bash
kubectl delete -f k8s-web-to-nginx.yaml -f nginx.yaml
```

```bash
kubectl delete all --all
```

### Deleting the cluster with minikube

In order to delete the cluster created for this project, you can run the following command:

```bash
minikube delete
```
