# oziuzoma-syself-devops

# Question 1 - Helm Chart to deploy a sample backend

Helm Chart for the backend application can be found in the repo called `samplebackend-helm-chart`. It deploys a NodeJS backend application which is a placeholder image for the purpose of this task. This backend application will be exposed on port 5000 via a ClusterIP service for internal cluster access only. 

`helm install release-name samplebackend-helm-chart/ --namespace syself --
create-namespace`

For Secret Management, an external secret manager or vault to store secrets and have them synced into the cluster. It is not best practice to push secrets into version control. For example, I have worked with Azure Keyvault where the controller is installed into the cluster then Keyvault resouces which reference the secrets in azure are created and synced to be used by pods. That is the general idea for secret management tools and a better way to handled kubernetes secrets. As the database becomes available, the deployment manifest in git will be updated to reflect the database connection infomation pulling from the synced secret. 

## There are a few considerations to make to selecting a database

Whether you want a self managed database or a fully managed database.

Whether your application needs a relational or non-relational database and which option to go for amongst the different engines avaialable.

I have seen some databases being hosted on Kubernetes as Statefulsets. However, it is not best practice because of the risk involved, sometimes the whole cluster could go down and having databases possibly being unavailable for sometime is not advisable. For my application, I choose to go with a fully managed database like AWS RDS or any cloud provider fully managed service. It provides automated backups and generally reduces overheads. If cost is an issue, then you can self host your databases on VMs so that your applications can connect to them. Read replicas will be provisioned for read only operations along with a standby primary/master for read and write operations for fault tolerance and HA.

Postgres DB will be provisoned because it is robust and I am more familiar with it.

 # Question 2 - Production-grade Kubernetes environment

For a self-managed kubernetes production environment. We can use the kubeadm tool to provision kubernetes on the VMs. We can an infrastructure as code tool like terraform or vagrant to provision our VMs to automate the process and mainatain uniformity across the environment. For high availabilty, we will provison 2 control planes and at least 3 worker nodes. All nodes will be provioned in the same private network and have the neccesary CPU, RAM and Storage requirements to match the type of appplications that are going to deployed onto Kubernetes.

Installing Kubernetes with kubeadm tool:

    1. Install containter runtime

    2. Install kubeadm , kubelet and kubectl

    3. Configure cgroup drivers

    4. Initialize the control plane using kubeadm init and passing the neccessary arguments

    5. Deploy a pod network (CNI plugin) like calico and weave-net especially if you plan to use network policies later for security

    6. Join the worker nodes using kudeadm join and token from the output from step 4.

    7. Now you have a cluster of 3 worker and 2 masters with kubernetes installed

 ### Control Plane Components

**etcd** - "Consistent and highly-available key value store used as Kubernetes' backing store for all cluster data." It should have backup in place and also high availabilty (at least 3 members).

**kube-apiserver** - "The API server is a component of the Kubernetes control plane that exposes the Kubernetes API. The API server is the front end for the Kubernetes control plane."

**kube-scheduler** - "Control plane component that watches for newly created Pods with no assigned node, and selects a node for them to run on."

**kube-controller-manager** - "Control plane component that runs controller processes.Logically, each controller is a separate process, but to reduce complexity, they are all compiled into a single binary and run in a single process."

 ### Worker Nodes Components

**kubelet** - "An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod."

**container runtime** - "A fundamental component that empowers Kubernetes to run containers effectively. It is responsible for managing the execution and lifecycle of containers within the Kubernetes environment."

**kube-proxy** - "A network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept."

### Storage 

A container storage interface (CSI) is a plugin that mediates between the underlying storage and the cluster such that pods can make use of external storage. The appropriate CSI should be installed into the cluster by following its documentation. Once installed, storage classes can be created and CSI can be specified as its provioner.

Storage Classes dynamically provision the persistent volumes and defines the requirements of the volume needed. Pods make a claim to these PVs via a persistent volume claim (PVC) which specifies the storage class. A pod makes its claim, the PV is created by the storage class and bound to the pod.

### Security

This layer defines authentication and authorization to the cluster. Users who can have access to the cluster and the kubeconfig they will use to access the cluster
and  what level of access they will have will depend on the roles, cluster roles, cluster role and role bindings. The difference between the role and cluster role is that the role is bound to a namespace while a cluster role isn't. By default, all pods can communicate with eachother however we can use network policies to restrict communication. Network poilices are only enforced by certain CNI like Calico. If you use an unsupported CNI, they will be created but will not enforce restrictions.


### Monitoring and Observability

We can provision open source tools like Prometheus, Grafana and Loki for continuous monitoring. Loki is for log aggregrationa dn can be configured to export the logs to a remote storage. Prometheus exports metrics from the nodes via a node exporter that will be installed on the nodes, it can also be integrated with an alert manager for alerts to notify the team via email/slack. Grafana is used to visualize the metrics, It will take loki and Prometheus as data sources and then use the metrics for various dashboards.

### Disaster Recovery and Backup

Regular snapshots of the nodes, etcd and all storage should be scheduled during a 
maintenance window. 

### Post Kubernetes Installation

A GitOps tool called ArgoCD can be used to deploy manifest files into the destination clusters. It is best practice to check on manifest into version control and not apply manifests directly via `kubectl`. Destination clusters are added to ArgoCD as kubernetes secrets and applications are created to sync resources. Application sets can also be used to automate the creation of ArgoCD applications.

 ## REFERENCES:

 [Helm Docs](https://helm.sh/docs/topics/charts/)

 [Kubernetes Docs](https://kubernetes.io/docs/home/)