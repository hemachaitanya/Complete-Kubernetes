### Kops

Virtual Machine setup:

```
vagrant init ubuntu/xenial64
```

Virtual Machine run:

```
vagrant up
```

Download Kops on Machine:

```
brew update && brew install kops
```

Install AWS CLI using pip. Create Kops user on AWS and give it Administrative Access.

Create a subdomain using Route53 for Kubernetes: (kubernetes.domain.com).

To create a kops cluster use:

```
kops create cluster --name=kubernetes.domain.com --state=s3://kobs-state-blah --zones=eu-west-1a --node-count=2 --node-size=t2.micro --master-size=t2.micro --dns-zone=kubernetes.domain.com
```

To configure the cluster use:

```
kops update cluster kubernetes.domain.com --yes
```

To edit the cluster use:

```
kops edit cluster kubernetes.domain.com
```

Or

```
kops edit cluster kubernetes.domain.com --state=s3://kops-state-blah
```

Docker/Kubernetes Remarks:

- You should only run one process in one container
  - Don't try to create one giant docker image for your app, split it up if necessary.
- All the data in the container is not preserved, when a container stops, all the changes within a container are lost.
  - You can preserve data, using volumes, which is covered later in this course

- A pod describes an application running on Kubernetes.
- A pod can contain one or more tightly coupled containers, that make up the app.
- Those apps can easily communicate with each other using their local port numbers.
- Our app only has one container.
- Create pod-helloworld.yml

```
apiVersion: v1
kind: Pod
metadata:
  name: nodehelloworld.example.com
  labels:
    app: helloworld
spec:
  containers:
  - name: k8s-demo
    image: my-image
  ports:
  - name: nodejs-port
  - containerPort: 3000
```

To create a pod-helloworld.yml with the pod definition use:

```
kubectl create -f k8s-demo/pod-helloworld.yml
```

Useful commands:

```
kubectl get pod - Get information about all running pods
kubectl describe pod <pod> - Describe one pod
kubectl expose pod <pod> --port=444 --name=frontend - Expose the port of a pod (creates a new service)
kubectl port-forward <pod> 8080 - Port forward the exposed post port to your local machine
kubectl attach <podname> -i - Attach to the pod
kubectl exec <pod> -- command - Execute a command on the pod
kubectl label pods <pod> mylabel=awesome - Add a new label to a pod
kubectl run -i --tty busybox --image=busybox --restart=Never -- sh - Run a shell in a pod - very useful for debugging
```