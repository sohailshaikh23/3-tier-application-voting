

# 3 tier Voting Application

This cloud-native voting application is built using a mix of technologies. It's designed to be accessible to users via the internet, allowing them to vote for their preferred programming language out of six choices: C#, Python, JavaScript, Go, Java, and NodeJS.

## Tier details

- **Frontend**: The frontend of this application is built using **React and JavaScript**. It provides a responsive and user-friendly interface for casting votes.

- **Backend / API**: The backend of this application is powered by **Go (Golang)**. It serves as the API handling user voting requests.

- **Database ** MongoDB is used as the database, configured with a replica set for data redundancy and high availability.


## Kubernetes Resources used for this project 

To deploy and manage this application efficiently and effectively, we use Kubernetes and its resources:

- **Namespace**: Kubernetes namespaces are utilized to create isolated environments for different components of the application, ensuring separation and organization.

- **Secret**: Kubernetes secrets store sensitive information, such as API keys or credentials, required by the application securely.

- **Deployment**: Kubernetes deployments define how many instances of the application should run and provide instructions for updates and scaling.

- **Service**: Kubernetes services ensure that users can access the application by directing incoming traffic to the appropriate instances.

- **StatefulSet**: For components requiring statefulness, such as the MongoDB replica set, Kubernetes StatefulSets are employed to maintain order and unique identities.

- **PersistentVolume and PersistentVolumeClaim**: These Kubernetes resources manage the storage required for the application, ensuring data persistence and scalability.


## Project Achievement / Outcomes

Creating and deploying this application with Kubernetes offers a valuable learning experience. Here are some Achievement:

1. **Containerization**: Gain hands-on experience with containerization technologies like Docker for packaging applications and their dependencies.

2. **Kubernetes Orchestration**: Learn how to use Kubernetes to efficiently manage, deploy, and scale containerized applications in a production environment.

3. **Microservices Architecture**: Explore the benefits and challenges of a microservices architecture, where the frontend and backend are decoupled and independently scalable.

4. **Database Replication**: Understand how to set up and manage a MongoDB replica set for data redundancy and high availability.

5. **Security and Secrets Management**: Learn best practices for securing sensitive information using Kubernetes secrets.

6. **Stateful Applications**: Gain insights into the nuances of deploying stateful applications within a container orchestration environment.

7. **Persistent Storage**: Understand how Kubernetes manages and provisions persistent storage for applications with state.

After working on this project, I have develop a deeper understanding of cloud-native application development, containerization, Kubernetes, and the various technologies involved in building and deploying such applications.


### **************************Steps to Deploy**************************



Step 1 :Create EKS cluster with NodeGroup (2 nodes of t2.medium instance type)
Step 2 :Create EC2 Instance t2.micro (For kubectl)

## USe this IAM role for ec2 (t2.micro)	
```
{
	"Version": "2012-10-17",
	"Statement": [{
		"Effect": "Allow",
		"Action": [
			"eks:DescribeCluster",
			"eks:ListClusters",
			"eks:DescribeNodegroup",
			"eks:ListNodegroups",
			"eks:ListUpdates",
			"eks:AccessKubernetesApi"
		],
		"Resource": "*"
	}]
}
```

step 3 :Install Kubectl on (t2.micro)	
```
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.24.11/2023-03-17/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo cp ./kubectl /usr/local/bin
export PATH=/usr/local/bin:$PATH
```

step 4 :Install AWScli:
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```


Once the Cluster is ready run the command to set connect EKS to your kubectl:
```
aws eks update-kubeconfig --name cluster1 --region ap-south-1
```


To check the nodes in your cluster run
```
kubectl get nodes
```


If using EC2 and getting "error : You must be logged in to the server (Unauthorized)", refer this: https://repost.aws/knowledge-center/eks-api-server-unauthorized-error

Solution for error : 

```
kubectl edit configmap aws-auth -n kube-system
```

add this data in the aws-auth or use AWS configure 

```
- rolearn: <YOUR ARN>
  username: EKSaccess
  groups:
	- system:masters
```



Clone the github repo
```
git clone https://github.com/sohailshaikh23/voting-app.git
```

**Create Namespace as voting_apps **

```
kubectl create ns voting_app
```
** set  this namespace as default **

```
kubectl config set-context --current --namespace voting_app

```


**MONGO Database Setup**


To create Mongo statefulset with Persistent volumes, use this manifests file:

```
kubectl apply -f mongo-statefulset.yaml
```

Mongo Service
```
kubectl apply -f mongo-service.yaml
```


Go inside mongo-0 pod to set replication
```
kubectl exec -it mongo-0 -- mongo
```

Run these commands while you inside to set replication (main) and secondary

```
rs.initiate(); 
sleep(2000);
rs.add("mongo-1.mongo:27017");
sleep(2000);
rs.add("mongo-2.mongo:27017");
sleep(2000);
cfg = rs.conf();
cfg.members[0].host = "mongo-0.mongo:27017";
rs.reconfig(cfg, {force: true});
sleep(5000);
```

Now check the status
```
rs.status()
``` 

create a database and run every command one by one

```
use langdb;
```

csharp
```
db.languages.insert({"name" : "csharp", "codedetail" : { "usecase" : "system, web, server-side", "rank" : 5, "compiled" : false, "homepage" : "https://dotnet.microsoft.com/learn/csharp", "download" : "https://dotnet.microsoft.com/download/", "votes" : 0}});
```

Python
```
db.languages.insert({"name" : "python", "codedetail" : { "usecase" : "system, web, server-side", "rank" : 3, "script" : false, "homepage" : "https://www.python.org/", "download" : "https://www.python.org/downloads/", "votes" : 0}});
```


JavaScript
```
db.languages.insert({"name" : "javascript", "codedetail" : { "usecase" : "web, client-side", "rank" : 7, "script" : false, "homepage" : "https://en.wikipedia.org/wiki/JavaScript", "download" : "n/a", "votes" : 0}});
```

Golang
```
db.languages.insert({"name" : "go", "codedetail" : { "usecase" : "system, web, server-side", "rank" : 12, "compiled" : true, "homepage" : "https://golang.org", "download" : "https://golang.org/dl/", "votes" : 0}});
```


Java
```
db.languages.insert({"name" : "java", "codedetail" : { "usecase" : "system, web, server-side", "rank" : 1, "compiled" : true, "homepage" : "https://www.java.com/en/", "download" : "https://www.java.com/en/download/", "votes" : 0}});
```

NodeJS
```
db.languages.insert({"name" : "nodejs", "codedetail" : { "usecase" : "system, web, server-side", "rank" : 20, "script" : false, "homepage" : "https://nodejs.org/en/", "download" : "https://nodejs.org/en/download/", "votes" : 0}});
```


CHECK YOUR INSERTED DATA 
```
db.languages.find().pretty();
```





**Backend/API Setup**


Create and apply Secrets files 
```
kubectl apply -f mongo-secret.yaml
```

Create and apply backend deploymeny files

```
kubectl apply -f api-deployment.yaml
```


Create and apply backend service files 
```
kubectl apply -f api-Service.yaml
```


check loadbalancer URL in browser with /ok , /languages, /languages/python



 **Frontend Setup**



Create and apply frontend deploymeny files

```
kubectl apply -f frontend-deployment.yaml
```


Create and apply frontend service files 
```
kubectl apply -f front-Service.yaml
```


check loadbalancer URL in browser 



































Test the full end-to-end cloud native application


Query MongoDB database directly to observe the updated vote data. In the terminal execute the following command:
```
kubectl exec -it mongo-0 -- mongo langdb --eval "db.languages.find().pretty()"
```

## **Summary**

In this Project, I have learnt how to deploy a cloud native application into EKS. Once deployed and up and running,we use browser to test out the application. You can confirmed that your activity within the application generated data which was captured and recorded successfully within the MongoDB ReplicaSet back end within the cluster.
