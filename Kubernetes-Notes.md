### High Availability

- `When we Run our Application in Docker Containers and if Containers fails we need to Manually Start the Conatiner(Container Down)`

- `If the Node i.e The machine Fails all the Container Running on the same machine it Should be Re-Created on Other machine(Node Down)`

- `the user should not have any Downtime`

- Example:
  - when your application is running in Docker Containers for some reasons in your Applications is gone Down that means your Container is Exited State or Stoped State. So Your application will have a Downtime. Now check my application is gone into the Exited or stoped state Now i would go probably Docker Container Run or Start Command are execute and Make the Conatainer Work

  - The problem is When your working in Single System . If there is Some Software Which Recogniges that When Containers goes Down it will automatically Do the Work So Starting Containers back So your application is always Up and Running. There is Some Application can Do that . It will help us

- `k8s Can do both the above But Docker which cannot do it. might Docker Swarm Does it`
  
### Auto Scalling

* Containers don’t scale on their own

Scaling there are two types

 - Vertical Scaling
 - Horizontal Scaling

- Vertical Scaling : `Increasing Size of the Conatiner`

- Example:
* intially you would give 256 Mb but storage not enough so now you will give 512 Mb
* intially you are giving 1 CPU and now you are giving 2 CPUs
  
- Horizontal Scalling: `Increasing number of Containers`
  
* You application is runnning in One Container Now you are going to Run 10 Containers

* K8s can do both Vertical Scaling and Horizontal Scaling of Conatiners
  
### Zero-Downtime Deployment

 * `k8s can handle Deployments with near Zero-Downtime Deployments`
 * `k8s can handle rollout(newverion) and rollback(Undo new version => Older Version)`

* Example:
Generally when we run on Application in it is not guarenteed that it could be the same containers running forever it will always get new realeses and then we moving from older Version into Newer Version we would want to Zero-Downtime or Near Zero-Downtime Deployments

### `k8s is Described as Production grade container Management`

### k8s History

* Google had a history of running everything on containers.
* To manage these containers, Google has developed container management tools (inhouse), they developed by `Borg`
* Omega is internal project developed by google in C++ to improve the shortcomings of Borg
* Google has started container orchestration platform as a open source and written in Go language and later donated the project to `CNCF` (Cloud Native Computing foundation)

