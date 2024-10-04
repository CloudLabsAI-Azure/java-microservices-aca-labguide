# Exercise 1: Plan a Java Application Migration to Azure Container App [read-only]

### Estimated Duration: 30 minutes

## Lab Scenario

You want to establish a plan for migrating your existing Spring Petclinic microservices application to Azure.

## Lab Objectives

After you complete this lab, you will be able to:

 - Examine the application components based on the information provided in its GitHub repository
 - Identify the Azure services most suitable for hosting your application
 - Identify the Azure services most suitable for storing data of your application
 - Identify how you organize resources in Azure

## Task 1: Examine the application components based on the information provided in its GitHub repository

#### System Overview

This microservices architecture is designed to manage a veterinary clinic's services, providing various functionalities through multiple, independently deployable components. Each service is built using Spring Boot, a popular framework for building Java-based applications, and they all interact seamlessly to deliver a scalable and modular solution. The system leverages a combination of service discovery, centralized configuration, API gateways, and monitoring to ensure reliability, scalability, and ease of management.

![](./media/overview.jpg)

#### Components:

 - **Customers Service:** The Customers Service manages pet owners and their pets' details. It allows for CRUD operations on pet owner records and pet profiles, providing essential information about pets and their owners to the rest of the system.

 - **Vets Service:** The Vets Service holds the information about veterinary specialists working in the clinic. It contains information such as a vet's name, specialization, available schedule, and other relevant details.

 - **Visits Service:** The Visits Service tracks all the visits and appointments scheduled between pets and veterinarians. It logs every vet visit, including the date, time, and medical notes taken during the visit.

 - **API Gateway:** The API Gateway serves as the entry point for all client requests, handling routing to the appropriate microservices. It consolidates access to the Customers Service, Vets Service, and Visits Service, ensuring that client requests are correctly routed based on the service they are targeting.

 - **Spring Cloud Config Server:** This server centralizes the configuration of all microservices in the system. Configuration files for each service (such as YAML or properties files) are stored in a Git repository, and each service fetches its configurations at runtime from this server.

 - **Eureka Service Discovery:** Eureka is responsible for service discovery. All services register themselves with Eureka and retrieve the IP addresses of other services dynamically. This allows for seamless scaling and management of services in the architecture.

 - **Spring Boot Admin Server:** This server is used to monitor and manage the microservices. It provides real-time information about each service’s health, memory usage, and other metrics. Administrators can use it to start, stop, or monitor the services in the system.

 ## Task 2: Consider the Azure services most suitable for hosting your application

 When migrating the Spring Petclinic application to Azure, there are several compute options available, each with its advantages. Below is an analysis of the four primary options: Azure App Service, Azure Kubernetes Service (AKS), Azure Container Apps, and Azure Spring Apps, along with a conclusion for the most suitable choice for this scenario.

 #### Azure App Service
 
 Azure App Service is a fully managed platform-as-a-service (PaaS) that allows developers to focus on building applications without worrying about the underlying infrastructure. It supports automatic scaling and updates, and it provides public endpoints for accessibility. However, while it simplifies deployment, it is primarily designed for monolithic applications. In a microservices architecture like Spring Petclinic, you would need to create separate web app instances for each microservice, leading to increased complexity in managing inter-service communication and scaling. Additionally, it lacks built-in support for Spring Cloud components, limiting its capability in a microservices environment.

 #### Azure Kubernetes Service (AKS)
 
 Azure Kubernetes Service (AKS) offers a highly customizable and flexible platform, allowing for granular control over microservices and their communication. It supports traffic control between services using network policies and provides a robust environment for deploying Spring Boot applications. However, AKS requires the containerization of all components, adding complexity to the migration process. Additionally, it necessitates provisioning and managing agent pools, which increases operational overhead. The need for an Azure Container Registry to store images also adds to the administrative burden, making it a more challenging option for teams unfamiliar with Kubernetes.

 #### Azure Spring Apps
 
 Azure Spring Apps is designed specifically for Spring Boot and Spring Cloud applications, making it an ideal migration path for applications built with these technologies. It offers a fully managed environment, taking care of infrastructure management. However, while it streamlines the deployment of Spring applications, it may introduce complexities for future containerized microservices that do not rely on Spring frameworks, and its pricing could be higher for non-Spring applications.

 #### Azure Container Apps
 
 Azure Container Apps is a fully managed service that simplifies the deployment and scaling of containerized applications. It supports microservices architecture and allows for automatic and independent scaling of each component, while eliminating the administrative overhead associated with managing Kubernetes clusters. With built-in support for Spring Boot and Spring Cloud components, it facilitates easy migration of existing applications like Petclinic.

#### Conclusion

Considering the requirements for the Spring Petclinic application, Azure Container Apps emerges as the most suitable choice for migration. It balances flexibility, simplicity, and cost-effectiveness while providing a fully managed environment for microservices. The service eliminates the need for managing Kubernetes clusters, allowing developers to focus on building and scaling their applications without the associated overhead. Additionally, its support for Spring Cloud components facilitates a seamless transition from the existing architecture. Overall, Azure Container Apps provides an ideal solution for managing the complexities of a microservices architecture while ensuring public accessibility and easy scalability.

## Task 3: Consider the Azure services most suitable for storing data of your application

Now that you identified the viable compute platforms, you need to decide which Azure service could be used to store the applications data.

#### Azure SQL Database

Azure SQL Database is a fully managed relational database that offers automatic scaling, high availability, and robust security. It integrates seamlessly with other Azure services, making it a strong choice for applications requiring a relational model. However, migrating to Azure SQL may necessitate code changes, and licensing costs can be higher compared to open-source solutions.

#### Azure Database for MySQL

Azure Database for MySQL is a managed service ideal for applications already utilizing MySQL. It provides high availability, automated backups, and flexible scaling options. Challenges include potential performance limitations under heavy loads and complexities in migrating advanced MySQL features.

#### Azure Cosmos DB

Azure Cosmos DB offers a multi-model database solution with global distribution and low-latency access. It is highly scalable and suitable for applications needing strong consistency models. However, its complexity and higher costs compared to traditional relational databases can pose challenges, particularly for teams unfamiliar with NoSQL.

#### Azure Database for PostgreSQL

Azure Database for PostgreSQL is known for its advanced data types and powerful querying capabilities. It supports flexible scaling and high availability, making it suitable for complex applications. The potential learning curve and compatibility issues during migration could be obstacles for teams new to PostgreSQL.

#### Conclusion

We have selected Azure Database for MySQL as the primary storage solution for the Spring Petclinic application, leveraging its seamless integration and familiarity. Additionally, Azure Database for PostgreSQL is available as an option, providing flexibility for future needs and enhancing our microservices architecture.

## Task 4: Consider resource organization in Azure

You now have a clear understanding of which Azure services you will have working together for the first stage of migrating of the Spring Petclinic application. Next, you need to plan how the resource will be organized in Azure.

#### How many resource groups will you be creating for hosting your Azure resources?

In Azure all resources that are created and deleted together typically should belong to the same resource group. In this case, since there is 1 application which provides a specific functionality, you can provision all resources for this application in a single resource group.

#### How will you configure networking for the application components?

In this lab, the networking for Azure Container Apps is configured using a pre-deployed virtual network (VNet) and dedicated subnet. By leveraging a pre-existing VNet, the Container Apps can operate within a secure network environment. This setup ensures that the subnet is exclusively reserved for the Container App environment, facilitating efficient communication and resource management. For optimal functionality, the subnet should adhere to the required sizing, with at least a /27 for workload profiles and a /23 for the consumption-only plan.

#### Are there any supporting services you would need for running the application?

For Azure Container Apps, you might need a container registry, depending on how you prefer to deploy your apps, this can be containerized and in that case you will need the container registry. Or, you can also choose to deploy from a jar file or from source code. In those latter 2 cases Azure Container Apps will handle the containerization for you.

## Database Selection Note:

This lab supports both **MySQL** and **PostgreSQL** for the Spring Petclinic application. Depending on your preference, please follow the appropriate steps below:

- **If you are using MySQL**: You can continue with the current flow of this lab. All necessary steps for setting up and connecting to Azure Database for `MySQL` will be covered.

- **If you are using PostgreSQL**: To proceed with configuring and deploying your application using Azure Database for `PostgreSQL`, please skip ahead directly to `PAGE 6` and follow the detailed instructions provided there to continue the lab.






