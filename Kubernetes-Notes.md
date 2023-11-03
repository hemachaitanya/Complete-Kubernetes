# k8s

### High Availability

- `When we Run our Application in Docker Containers and if Containers fails we need to Manually Start the Conatiner(Container Down)`

- `If the Node i.e The machine Fails all the Container Running on the same machine it Should be Re-Created on Other machine(Node Down)`

- `the user should not have any Downtime`

- Example:
  - when your application is running in Docker Containers for some reasons in your Applications is gone Down that means your Container is Exited State or Stoped State. So Your application will have a Downtime. Now check my application is gone into the Exited or stoped state . Now i would go probably Docker Container Run or Start Command are execute and Make the Conatainer Work

  - The problem is When your working in Single System . If there is Some Software Which Recogniges that When Containers goes Down it will automatically Do the Work So Starting Containers back So your application is always Up and Running. There is Some Application can Do that . It will help us

- `k8s Can do both the above But Docker which cannot do it. might Docker Swarm Does it`
  
### Auto Scalling

* pod's don’t scale on their own

Scaling there are two types

 - Vertical Scaling
 - Horizontal Scaling

- Vertical Scaling : `Increasing Size of the Pods`

- Example:
* intially you would give 256 Mb but storage not enough so now you will give 512 Mb
* intially you are giving 1 CPU and now you are giving 2 CPUs
  
- Horizontal Scalling: `Increasing number of Pods`
  
* You application is runnning in One Container Now you are going to Run 10 Containers

* K8s can do both Vertical Scaling and Horizontal Scaling of Conatiners
  
### Zero-Downtime Deployment

 * `k8s can handle Deployments with near Zero-Downtime Deployments`
 * `k8s can handle rollout(newverion) and rollback(Undo new version => Older Version)`

* Example:
Generally when we run on Application in it is not guarenteed that it could be the same containers running forever it will always get new realeses and then we moving from older Version into Newer Version we would want to Zero-Downtime or Near Zero-Downtime Deployments

* `k8s is Described as Production grade container Management` 

### k8s History

* Google had a history of running everything on containers.
* To manage these containers, Google has developed container management tools (inhouse), they developed by `Borg`
* `Omega` is internal project developed by google in C++ to improve the `shortcomings of Borg`
* Google has started container orchestration platform as a open source and written in Go language and `later donated the project to "CNCF"` (Cloud Native Computing foundation)
* ` K8s is distributed application which is called as k8s cluster`  

# k8s architecture

- Control Plane or Master Node
  * kube-api Server
  * kube-scheduler
  * kube-controller-manager
  * etcd
* Worker Node or Slave
  * kubelet
  * container engine
  * kube-proxy
  
**ectd** 
* `This is memory of the k8s cluster or k8s uses etc to store all the cluster data`
* etcd cluster can scale across multiple nodes unlike traditional databases
* This is distribute key-value store
  
**kube-apiserver**
* `it will handels all communication of k8s cluster.let it be internal or external`
* This is most important component of the k8s control plane
* kube-api server exposes REST API which enables clients to send HTTP requests to kube-api server
* kube-api server responds over http requests and writes the resource information to etcd store
* kube-api server exposes the k8s objects
* kube-api server is responsible for all the communication
* The api server is over https and requires authentication
  
**kube-scheduler**
* `Scheduler is responsible for creating k8s objects(pods) and scheduling them on right node`

**kube-controller-manager**
* `This ensures desired state is maintained`
* for creating / updating pods and other objects
* This is combination of multiple controller
  - NodeController
  - Replication Controller
  - Namespace Controller
  - EndpointController
  - ServiceAccountController
kubelet
- `This is the agent of the control plane` 
- This reacts to requests/orders from control plane components and speaks with container runtime and gets the work done
- If it fails responds back to control plane with status

**kube-proxy**

- this is responsible for networking
- this implementations will be based on CNI

**Container Runtime**:

- this is container engine
- this could be docker or any other CRI compatible runtime
  
##  Pod lifecycle or States of a Pod

**Running**:Once all the containers in the POD starts, it goes in to running state.

**Pending**:When the Pod’s metadata is accepted by the Kuberenetes but still hasn’t been deployed to any node, Pod goes into the Pending state.
  -  E.g. When Nodes do not have enough resources in the cluster, it causes the pod to go in pending state.
  
**Succeeded**:When containers inside the the pod get terminated with exit code 0 then the pod goes into the Succeeded State.

**Failed**:When one of the containers inside the pod gets terminated with exit code other than 0, It causes the pod to go into the Failed state.

**unknown**:The Pod status couldn’t be obtained by the API server.

### CrashLoopBackoff
- When the containers inside the pod get failed to start then the pod is being recreated again and again.
- The apllication inside the container keeps crashing
- some type of parameters of the pod or container have been configured incompletely
   - example: Environmental varibles, parameters
- An error have been made when deploying k8s

#### Management approaches
-   **Imperative commands**: `run`, `expose`, `scale`, `edit`, `create deployment`
-   **Imperative objects**: `create -f file.yml`, `replace -f file.yml`, ...
-   **Declarative objects**: `apply -f file.yml`, or `dir\`, `diff`
    -   Best for prod, easier to automate
    -   Harder to understand and predict changes
-   Most Important Rule:
    -   **Don't mix the three approaches!**
-   Move to `apply -f file.yml` and `apply -f directory\` for prod
-   Store yaml in git, git commit each change before you apply
-   This trains you for later doing GitOps (where git commits are automatically applied to clusters)

### Declarative Kubernetes YAML
-   Focus on the YAML or JSON file
-   Create/update resources in a file: `kubectl apply -f filename.yml`
-   Create/update a whole directory of yaml: `kubectl apply -f myyaml\`
-   Create/update from a URL: `kubectl apply -f https://bret.run/pod.yml`
    -   Be careful! Look at the file first!
-   Each file contains one or more manifests
-   Each manifest describes an API object (deployment, job, secret)
-   Each manifest needs four parts (root key:values in the file)
    -   `apiVersion:`
    -   `kind:`
    -   `metadata:`
    -   `spec:`
**apiVersion**:what type of version the `Kubernetes API used to create the object`
**kind**: what type of Kubernetes resource you are creating. `It specifies the kind of object you want to create`
  -  example: Pod,Deployment,Service,ConfigMap, etc.   
  -   `kubectl api-resources`
-   `apiVersion`:
    -   `kubectl api-versions`
    -   Old api version with new cluster can result in some issues with deploying
**metadata**:metadata that helps `uniquely identify the object`, including a name and optional namespace
**spec**: `configuration that defines the desired for the object`

#### Building the YAML spec
-   `kubectl explain services --recursive`: getting all the keys each kind support
-   Drilldown example:
    -   `kubectl explain services.spec`
    -   `kubectl explain services.spec.type`
    -   `kubectl explain deployment.spec.template.spec.volumes.nfs.server`
-   We can also see api-referance docs --> Kubernetes API reference
-   `kubectl diff -f app-yml`: visually showing the difference between the specs from latest (changed) YAML file and current specs on disk!


### some popular k8s workloads

- Pods
- ReplicaSets
- Deployments
- StatefulSets
- DaemonSets
- Jobs and CronJobs
- Stateless Services
- Stateful Services
- Services and EndPoints

### **Pod**: 
  - one or more containers running together on one Node
  - smallest unit of k8s cluster that can be created, schedule, and managed on a Kubernetes cluster. Each pod is assigned a `unique IP address` within the cluster. Containers are always in pods.
  - Kubernetes doesn’t run containers directly; instead it wraps one or more containers into a higher-level structure called a pod
  - each pod has unique ip address
  - IP addresses are associated with pods, not with individual containers
  - Containers in a pod share `localhost`, and can share volumes (containers in the same pod can communicate via localhost)

- Example: 
```yml
apiVersion: v1 # what type of version you are using Kubernetes API
kind: Pod # what type of resource you have created
metadata:
  name: k8s1 # unique identification of the name
  labels:
    env1: dev # querry the information
spec: #where you define the specification for the Pod, including its containers.
  containers:
    - name: nop #name of the container
      image: batchusivaji/nopcommerce:latest #specifies the name of the image to use
      ports:
        - containerPort: 5000 # which port are you run your application 

```



