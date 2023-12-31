## Jenkins: An Overview

[Jenkins](https://www.jenkins.io/) is an open-source automation server that facilitates continuous integration (CI) and continuous delivery (CD) of software projects. It helps streamline the development process by automating various tasks related to building, testing, and deploying software applications. Jenkins is highly customizable and extensible through a vast collection of plugins, making it suitable for a wide range of development environments and workflows.

### Key Features of Jenkins

1. **Continuous Integration (CI):** Jenkins allows developers to integrate their code changes into a shared repository on a frequent basis. It automatically triggers build processes, compiles the code, and runs tests whenever new code is pushed to the repository. This early feedback helps identify and fix issues quickly.

2. **Continuous Delivery/Deployment (CD):** Jenkins can be used to automate the process of deploying applications to various environments (development, testing, staging, production, etc.). It ensures that the deployment process is repeatable, consistent, and reliable, reducing the chances of errors during deployment.

3. **Automation:** Jenkins enables the automation of various tasks, such as building, testing, and packaging applications. This automation helps save time and ensures consistency in the development and deployment processes.

4. **Extensibility:** Jenkins provides a vast array of plugins that extend its functionality. These plugins cover a wide range of tasks, from source code management and build tools to testing frameworks and deployment technologies. Users can customize and configure Jenkins to suit their specific needs.

5. **Distributed Builds:** Jenkins supports distributed builds, allowing users to distribute build and test workloads across multiple machines or nodes. This helps optimize resource usage and reduces build times.

6. **Easy Configuration:** Jenkins offers a web-based interface for configuring jobs and pipelines. Users can define complex workflows through a graphical interface or by writing code in domain-specific languages like Groovy.

7. **Pipeline Support:** Jenkins supports defining and managing complex automation workflows as code using Jenkins Pipelines. This allows teams to version control their build and deployment processes, making them more manageable and reproducible.

8. **Monitoring and Reporting:** Jenkins provides various tools and plugins for monitoring build and deployment processes. It generates reports, logs, and notifications to help developers and teams keep track of the status and progress of their projects.

9. **Security:** Jenkins supports authentication and authorization mechanisms to control access to its features and resources. It allows users to define roles, permissions, and access controls to ensure the security of the automation server.

10. **Community and Support:** Jenkins has a large and active community of developers, users, and contributors. This community contributes to the ongoing development of Jenkins and provides support through forums, documentation, and resources.

In summary, Jenkins is a powerful automation server that plays a crucial role in modern software development practices, enabling teams to automate repetitive tasks, accelerate the development cycle, and ensure the quality and reliability of their software applications.

For more information, visit the [Jenkins website](https://www.jenkins.io/).


### Prerequisites

Before we start it is important to make sure that all the prequsities have been met

1. **EC2 Instance:** For this the very first step is to have 2 EC2 machine active, one machine will be our production machine and the other machine will be Jenkins server.

2. **Installing Jenkins:** On the Jenkins Machine, Install Jenkins : https://www.jenkins.io/doc/book/installing/ . Follow the offical webiste step to do it.

3. **Confirm App gets Deployed Sucesfully:** In the production Machine make sure that node and other necessary dependencies is installed. Have a manual deploymenet first just to confirm everything is working in place.

### Implementing Jenkins Pipeline
1. **Setting up Jenkins** : We need to set up Jenkins, for that refer [this](https://github.com/thomasjv799/Devops_Training/blob/main/4_Jenkins/Files/1_HowtoConfigureAWSEC2InstanceandInstallJenkins.pdf)

  
2. **Storing the Keys in respective Machines and other configurations:** From the production Machine, we need to get the SSH Public Key and Private Key. The Public Key must be stored in our Github account, this is so that we can clone out repo with authentication. Similarly in Jenkins Machine we need to store both the SSH Private Key and Machine Authentication key which is the .pem/.putty file that gets generated while creating the ec2 instance. Also we need to install a SSH agent extension and set [Github Webhook](https://docs.github.com/en/webhooks-and-events/webhooks/about-webhooks) which is used for communication between Jenkins and Github. Detailed steps have been done,  Refer [here](https://github.com/thomasjv799/Devops_Training/blob/main/4_Jenkins/Files/2_HowtoConfigureJenkinsCredentialsandWebhookNotifications.pdf)


![alt-text](https://github.com/thomasjv799/Devops_Training/blob/main/4_Jenkins/Files/Set_Up.gif)


3. **Setting up and Running the Pipeline:** Finally we need to run our pipeline, the details for this step is at [here](https://github.com/thomasjv799/Devops_Training/blob/main/4_Jenkins/Files/3_SettingUpJenkinsPipelineforReactSampleAppDeployment.pdf)
