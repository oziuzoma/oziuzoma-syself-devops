# oziuzoma-syself-devops

# Question 1 - Helm Chart to deploy a sample backend

Helm Chart for the backend application can be found in the repo called samplebackend-helm-chart. It deploys a nodejs backend application which is a placeholder image for the purpose of this task. Since it is a backend application it will be exposed via a ClusterIP type service for internal cluster exposure only on port 5000. For Secret Management, I will use an external secret manager or vault to store secets and have them synced into the cluster. It is not best practice to push secrets into version control.For example, I have worked with azure key vault where the controller is installed into the cluster then we create azure key vault resouces which reference the secrets in azure and have them synced into the cluster to be used by pods/deployments.That is the general idea for secret management tools and a better way to handled kubernetes secrets.

## There are a few considerations to make to selecting a database:

Whether you want a self managed database or a fully managed database.

whether your application needs a relational or non-relational database and which option to go for amongst the different engines avaialable.

 I have seen some databases being hosted on Kubernetes as stateful sets. However it is not best practice because of the risk involved, though it rarely happens sometimes the whole cluster could go down and having important infomation in your database possibly being unavailable for sometime is not advisable. For my application  choose to go with a fully managed database like AWS RDS or any cloud provider fully managed sevrice to manage the database externally. RDS provides automated backups and generally redcues overheads. If cost is an issue, then you can self host your databases on VMs so that your application can connect to them. Read replicas will be provisioned for read only along with a standby primary/master for read and write operations for fault tolerance

 I will also pick postgres as my preferred engine because i am more familiar with it.

 # Question 2 - Production-grade Kubernetes environment

 For a self-managed kubermetes production environment. We can use the kubeadm tool to provision kubernetes on the VMs. We can an infrastructure as code tool like terraform or vagrant to provision our VMs to automate the process and mainatain uniformity acrooss the environment. In order for high availabilty we will provison 2 control planes and at least 3 worker nodes. All nodes will be provioned in the same private network.

 On a high level to install kubernetes with kubeadm:

    1. Install containter runtime

    2. Install kubeadm , kubelet and kubectl

    3. Configure cgroup drivers

    4. Initialize the control plane using kubeadm init and passing the neccessary arguments

    5. Deploy a pod network (CNI plugin) like calico and weave-net especially if you plan to use network policies later for security

    6. Join the worker nodes using kudeadm join and token from the output from step 4.

    7. Now you have a cluster of 3 worker and 2 masters with kubernetes installed

 ### Control Plane Components:
    ** etcd ** - "Consistent and highly-available key value store used as Kubernetes' backing store for all cluster data." It should have backup in place and also high availabilty (at least 3 members).

    **kube-apiserver** - "The API server is a component of the Kubernetes control plane that exposes the Kubernetes API. The API server is the front end for the Kubernetes control plane."

    **kube-scheduler** - "Control plane component that watches for newly created Pods with no assigned node, and selects a node for them to run on."

    **kube-controller-manager** - "Control plane component that runs controller processes.Logically, each controller is a separate process, but to reduce complexity, they are all compiled into a single binary and run in a single process."

 ### Worker Nodes Components:

    **kubelet** - "An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod."

    **container runtime** - "A fundamental component that empowers Kubernetes to run containers effectively. It is responsible for managing the execution and lifecycle of containers within the Kubernetes environment."

    `kube-proxy` - "A network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept."



 ## REFERENCES:

 [Helm Docs](https://helm.sh/docs/topics/charts/)

 [Kubernetes Docs](https://kubernetes.io/docs/home/)