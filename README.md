# oziuzoma-syself-devops

# Question 1 - Helm Chart to deploy a sample backend

Helm Chart for the backend application can be found in the repo called samplebackend-helm-chart. It deploys a nodejs backend application which is a placeholder image for the purpose of this task.For secret management, i will be using a external secret manager or vault to store secets and have them synced into the cluster. It is not best practice to push secrets into version control.For example, I have worked with azure key vault where the controller is installed into the cluster then we create azure key vault resouces which reference the secrtes in azure and have them synced into the cluster to be used by pods/deployments.That is the genral idea for secret management tools and a better way to handled kubernetes secrets. my deployment will be referencing a secret called "backend-secret" which will be synced into the cluster this secret contains the databses connection information hat the backend will use to connect to the database.

## There are a few considerations to make to selecting a database:

Whether you want a self managed database or a fully managed database.

whether your application needs a relational or non-retional database and which option to go for amongst the different engines avaialable.

 I have seen some databases being hosted on Kubernetes as stateful sets. However it is not best practice because of the risk involved, though it rarely happens sometimes the whole cluster could go down and having important infomation in your database possibly being unavailable for sometime is not advisable. For my application  choose to go with a fully managed database like AWS RDS or any cloud provider fully managed sevrice to manage the database externally. RDS provides automated backups and generally redcues overheads. If cost is an issue, then you can self host your databases on VMs so that your application can connect to them.

 I will also pick postgres as my preferred engine because i am more familiar with it.

 # Question 2 - Helm Chart to deploy a sample backend




 ## REFERENCES:

 [Helm Docs](https://helm.sh/docs/topics/charts/)

 [Kubernetes Docs](https://kubernetes.io/docs/home/)