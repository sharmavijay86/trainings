## DCA
  ### Docker Architecture (Engine)
   - Setup on ubuntu and centos
	 - Basic container operation
	 - inspecting container
	 - stopping and removing container
	 - Setting hostname
	 - Restart policies
	 - Copying content into and out in container
	 - publishing port
	 - Docker debug mode
	 - Logging drivers 
  ### Docker Images
  - Docker Images registry
	- image tags and naming conventions 
	- authenticating against registries
	- inspecting and removing docker image
	- Save and Load images
	- Registry operations
	- Dockerfile  and docker build
	- build httpd image
	- COPY vs ADD  & CM vs entrypoint
	- Dockerfile best practices
  ### Docker Security
    - namespace and capabilities
	  - cgroups
	  - resource limits cpu and memory
 ### Docker networking
    -  Docker networking Commands
	  - namespaces and networking deep dive
 ### Docker Storage
    - Storage and Volume
 ### Docker-compose
    -  Microservice vs monolithic
	  - example of voting application 
 ### Docker swarm
   - Container orchestration
	 - swarm architecture
	 - swarm setup with 2 node cluster
	 - swarm operations
	 - swarm high availability - quorum
	 - Auto lock 
	 - swarm service
	 - scaling rolling updates and rollbacks
	 - swarm service types
	 - docker config objects
	 - docker overlay networking
	 - swarm service discovery
	 - Docker stack
 ### Kubernetes
   -  kubernetes architecture
	 -  pods
	 - pods with yaml and cmd
	 - replication controllers, replica setup
	 - deployments , Updates and rollbacks
	 - networking in kubernetes
	 - services , nodeport,clusterIP, loadbalancer
	 - Deploy a microservice example application
	 - Namespaces
	 - Environment variables and commands, comparison with docker
	 - Configmaps and secrets
	 - readiness and liveness probes
	 - Network policies
	 - Volumes in kubernetes
	 - PV, PVC and storage class
 ### Docker Engine enterprise
    - Deployment of EE 
	  - rbac
    - Docker Trusted registry
	  - access control in DTR
	  - image scanning
	  - image promotion
	  - garbage collection
## Kubernetes:
  ### Cluster architecture
    - Manage role based access control (RBAC)
	  - Use Kubeadm to install a basic cluster
	  - Manage a highly-available Kubernetes cluster
	  - Provision underlying infrastructure to deploy a Kubernetes cluster
	  - Perform a version upgrade on a Kubernetes cluster using Kubeadm
	  - Implement etcd backup and restore
    - Workloads & scheduling
	  - Understand deployments and how to perform rolling update and rollbacks
	  - Use ConfigMaps and Secrets to configure applications
	  - Know how to scale applications
	  - Understand the primitives used to create robust, self-healing, application deployments
	  - Understand how resource limits can affect Pod scheduling
	  - Awareness of manifest management and common templating tools
  ### Services & networking
  - Understand host networking configuration on the cluster nodes
	- Understand connectivity between Pods
	- Understand ClusterIP, NodePort, LoadBalancer service types and endpoints
	- Know how to use Ingress controllers and Ingress resources
	- Know how to configure and use CoreDNS
	- Choose an appropriate container network interface plugin
  ### Storage
  - Understand storage classes, persistent volumes
	- Understand volume mode, access modes and reclaim policies for volumes
	- Understand persistent volume claims primitive
	- Know how to configure applications with persistent storage
  ### Troubleshooting
	- Evaluate cluster and node logging
	- Understand how to monitor applications
	- Manage container stdout & stderr logs
	- Troubleshoot application failure
	- Troubleshoot cluster component failure
	- Troubleshoot networking
## CKAD
   ### Core concept
		- Kubernetes API
		- Create and configure basic pods
   ### Configuration
		- configmaps and secrets
		- security contexts
		- defining application resource requirements
		- understanding service accounts
###	multicontainer pods
		- understanding multicontainer pod design pattern( adapter, sidecar, ambassador )
###	Observability
	-	liveness readiness
	-	container logging
	-	application monitoring
	-	debugging applications
###	POD Design
	-	deployments and rolling updates
	-	jobs and cronjobs
	-	use of labels and selectors and annotations
###	State persistent
	-	PV and PVC and storage class
###	Services and networking
	-	servicesnetwork polices
