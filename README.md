# oziuzoma-syself-devops

#Question 1 - Helm Chart to deploy a sample backend

Helm Chart for the backend application can be found in the repo called samplebackend-helm-chart.

##There are a few considerations to make to selecting a database:

Whether you want a self managed database or a fully managed database
whether your application needs a relational or non-retional database and which option to go for amongst the different engines avaialable.

 I have seen some databases being hosted on Kubernetes as stateful sets.However i personally will not go for that option because of the risk involved, even though it rarely happens sometimes the whole cluster could go down and having important infomation in your database possibly being unavailable for sometime is not advisable. For my application  choose to go with a fully managed database like AWS RDS or any cloud provider fully managed sevrice to manage the database externally. RDS provides automated backups and generally redcues overheads. If cost is an issue, then you can self host your databases on VMs so that your application can connect to them.

 I will also pick postgres as my preferred engine because i am more familiar with it.

 #Question 2 - Helm Chart to deploy a sample backend




 #REFERENCES:

 [Helm Docs](https://helm.sh/docs/topics/charts/)
 [Kubernetes Docs] (https://kubernetes.io/docs/home/)