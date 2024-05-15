

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


