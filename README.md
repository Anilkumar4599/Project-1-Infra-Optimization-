# Project-1-Infra-Optimization

Create a DevOps infrastructure for an e-commerce application to run on high-availability mode.



B-Safe.
Course-end Project 3
DESCRIPTION
Create a CI/CD Pipeline to convert the legacy development process to a DevOps process.
Background of the problem statement:
A leading US healthcare company, Aetna, with a large IT structure had a 12-week release cycle and their business was impacted due to the legacy process. To gain
true business value through faster feature releases, better service quality, and cost optimization, they wanted to adopt agility in their build and release process.
The objective is to implement iterative deployments, continuous innovation, and automated testing through the assistance of the strategy.
Implementation requirements:
1.	Install and configure the Jenkins architecture on AWS instance
2.	Use the required plugins to run the build creation on a containerized platform
3.	Create and run the Docker image which will have the application artifacts
4.	Execute the automated tests on the created build
5.	Create your private repository and push the Docker image into the repository
6.	Expose the application on the respective ports so that the user can access the deployed application.
7.	Remove container stack after completing the job
The following tools must be used:
1.	EC2
2.	Jenkins
3.	Docker
4.	Git
The following things to be kept in check:
1.	You need to document the steps and write the algorithms in them.
2.	The submission of your Github repository link is mandatory. In order to track your tasks, you need to share the link of the repository.
3.	Document the step-by-step process starting from creating test cases, the executing it, and recording the results.
4.	You need to submit the final specification document, which includes:
•	Project and tester details
•	Concepts used in the project
•	Links to the GitHub repository to verify the project completion
•	Your conclusion on enhancing the application and defining the USPs (Unique Selling Points)

Step 1: Provision an EC2 Instance for Jenkins:

•	Log in to your AWS Management Console and navigate to the EC2 service.

•	Launch an EC2 instance using the Amazon Machine Image (AMI) that fits your requirements. Choose an appropriate instance type, storage, and security group settings.

•	Configure the instance details, such as VPC, subnet, and IAM role.

•	Review the settings and launch the instance. Make sure to create or select an existing key pair to access the instance securely.

•	Wait for the instance to be in a "running" state and note down its public IP address or DNS name.






































Step 2: Install and Configure Jenkins on the EC2 Instance:

SSH into the EC2 instance using the key pair you selected during instance provisioning.

Update the system packages:

sql
Copy code
sudo apt update
Install Jenkins by following the official documentation for your operating system. Here are general steps for Ubuntu:

ruby
Copy code
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins
Start the Jenkins service:

sql
Copy code
sudo systemctl start jenkins
Open a web browser and access Jenkins by entering the public IP address or DNS name of the EC2 instance followed by port 8080 (e.g., http://<EC2_Public_IP>:8080).

Retrieve the Jenkins initial admin password by running the following command on the EC2 instance:

bash
Copy code
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Copy the password and paste it into the Jenkins web interface to complete the setup wizard.

Install the necessary Jenkins plugins required for the project, such as Git plugin, Docker plugin, and any other plugins specific to your project needs. Follow the Jenkins documentation to install plugins.

Configure Jenkins global settings, including system configuration, security settings, and credentials for Git repository access.












Step 3: Set Up Git Repository and Webhook:

1.	Set up a Git repository for your application:
•	Choose a version control platform like GitHub, GitLab, or Bitbucket.
•	Create a new repository on the chosen platform.
•	Configure the repository settings according to your needs, such as access control (public or private), branch structure (default branch, protected branches), and any other repository-specific settings.
2.	Generate SSH keys on the Jenkins server:
•	SSH into the Jenkins server or access it through a terminal.
•	Run the following command to generate an SSH key pair:
cssCopy code
ssh-keygen -t rsa -b 4096 -C "your_email@example.com" 
•	You can specify a different email address if needed.
•	Choose a location to save the SSH keys, or press Enter to use the default location.
3.	Add the Jenkins server's public key to the Git repository:
•	Access the Git repository's settings or repository-specific SSH key configuration.
•	Retrieve the public key from the Jenkins server by running the following command on the Jenkins server terminal:
bashCopy code
cat ~/.ssh/id_rsa.pub 
•	Copy the entire content of the public key.
4.	Add the public key to the Git repository:
•	In the repository settings, locate the section for SSH keys or deploy keys.
•	Add a new deploy key or paste the Jenkins server's public key in the provided field.
•	Save the SSH key.
5.	Create a webhook on the Git repository:
•	Access the repository settings or webhook configuration in the Git platform.
•	Locate the option to create a new webhook.
•	Set the webhook URL to match your Jenkins server's webhook endpoint. It should be in the format: http(s)://<Jenkins_Server_IP>:<Jenkins_Port>/github-webhook/.
•	Configure the webhook to trigger on repository changes, such as push events or specific branch updates.
•	Save the webhook configuration.















Step 4: Create a Jenkins Pipeline Script:
Step 4: Create a Jenkins Pipeline Script:

In the Jenkins web interface, click on "New Item" to create a new Jenkins pipeline job.
Provide a name for the job and select "Pipeline" as the job type.
Scroll down to the "Pipeline" section and select "Pipeline script" or "Pipeline script from SCM" as the definition.
If you choose "Pipeline script", you can directly write the pipeline script in the Jenkins web interface.
If you choose "Pipeline script from SCM", you need to have a Jenkinsfile stored in your Git repository.
If you selected "Pipeline script from SCM":
Specify the repository URL, credentials, and branch where the Jenkinsfile is located.
You can also specify a specific script path if the Jenkinsfile is located in a subdirectory.
If you selected "Pipeline script":
In the script editor, you can directly write the Jenkins pipeline script using the Jenkins Pipeline DSL or use the visual editor if you're using Jenkins Blue Ocean.
Define stages and steps in the pipeline script.
Use the appropriate plugins and Docker-related steps to build, test, and deploy the application on a containerized platform.
For example, you can use the "checkout" step to fetch the source code from the Git repository, the "docker.build" step to build the Docker image, and additional steps for running automated tests.
Save the pipeline configuration.
Run the pipeline job by clicking on "Build Now" or wait for the webhook trigger if you have set it up.
The pipeline script will be executed according to the defined stages and steps, performing build, test, and deployment tasks.
Here's an example of a simplified Jenkins pipeline script:

groovy
Copy code
pipeline {
  agent any
  
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    
    stage('Build') {
      steps {
        sh 'docker build -t myapp .'
      }
    }
    
    stage('Test') {
      steps {
        sh 'docker run myapp npm test'
      }
    }
    
    stage('Deploy') {
      steps {
        sh 'docker run -d -p 8080:80 myapp'
      }
    }
  }
  
  post {
    always {
      sh 'docker rm -f $(docker ps -aq)'
    }
  }
}
This example uses the checkout step to fetch the source code from the Git repository, the docker build step to build the Docker image, the docker run steps to run tests and deploy the application, and the docker rm step to remove the container stack after completing the job.

Please note that this is a simplified example, and you may need to modify it according to your specific project requirements. Make sure to adjust the Docker build, test, deploy steps, and any other necessary configurations to match your setup.

Feel free to customize this pipeline script further based on your project's specific requirements and configurations.



























Step 5: Build and Push Docker Image:

Define a Dockerfile: Create a Dockerfile in your project's repository. The Dockerfile defines the steps to build the Docker image containing your application artifacts. Include any necessary dependencies and build steps. Here's a simplified example of a Dockerfile:

Dockerfile
Copy code
# Use a base image
FROM <base_image>

# Set the working directory
WORKDIR /app

# Copy the application files to the working directory
COPY . /app

# Install dependencies and perform any necessary build steps
RUN <command_to_install_dependencies>
RUN <command_to_build_application>

# Set the container command
CMD <command_to_start_application>
Replace <base_image> with the appropriate base image for your application and update the commands according to your specific build requirements.

Update Jenkins pipeline script: In your Jenkins pipeline script (Jenkinsfile), add the necessary steps to build and tag the Docker image.

groovy
Copy code
pipeline {
  // ...
  
  stages {
    // ...
    
    stage('Build and Push Docker Image') {
      steps {
        // Build the Docker image
        sh 'docker build -t my-registry/myapp .'
        
        // Tag the Docker image
        sh 'docker tag my-registry/myapp my-registry/myapp:latest'
        
        // Push the Docker image to the private repository
        withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
          sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD my-registry'
          sh 'docker push my-registry/myapp:latest'
        }
      }
    }
  }
  
  // ...
}
Update my-registry with your private repository details. Adjust the image name and tag (my-registry/myapp:latest) according to your project's naming conventions.

Configure Docker credentials: In the Jenkins web interface, navigate to "Manage Jenkins" > "Manage Credentials". Add a new username and password credential entry with the appropriate credentials for your private Docker repository. Take note of the credentialsId used in the Jenkins pipeline script.

Alternatively, you can use a Jenkins credential plugin specific to your Docker registry, such as the Docker Commons Plugin or Docker Hub Plugin, to manage Docker credentials directly in Jenkins.
Save and run the pipeline: Save the pipeline configuration and trigger a build manually or wait for the webhook trigger. The pipeline will execute the defined stages and steps, including building the Docker image and pushing it to your private repository.

By following these steps, you'll define a Dockerfile to build the Docker image, update the Jenkins pipeline script to include Docker-related steps, and configure Docker credentials to push the image to your private repository. Make sure to adapt the commands and configurations based on your project's specific requirements.


























Step 6: Expose the Application on Respective Ports

Update Jenkins pipeline script: In your Jenkins pipeline script (Jenkinsfile), add a step to deploy the Docker image on a container and configure the container to expose the required ports for accessing the application.

groovy
Copy code
pipeline {
  // ...
  
  stages {
    // ...
    
    stage('Deploy') {
      steps {
        // Run the Docker container
        sh 'docker run -d -p 8080:80 my-registry/myapp'
      }
    }
  }
  
  // ...
}
Update the -p 8080:80 option in the docker run command to specify the port mapping. This example maps port 8080 of the host machine to port 80 of the container. Adjust the port numbers according to your application's requirements.

Save and run the pipeline: Save the pipeline configuration and trigger a build manually or wait for the webhook trigger. The pipeline will execute the defined stages and steps, including deploying the Docker image on a container and exposing the application on the respective ports.

By following these steps, you'll add a step in the Jenkins pipeline script to deploy the Docker image on a container and configure the container to expose the required ports for accessing the deployed application. Make sure to adapt the port mapping according to your application's specific requirements.















Step 7: Remove Container Stack:

Update Jenkins pipeline script: In your Jenkins pipeline script (Jenkinsfile), add a post-build step to remove the container stack after completing the job.

groovy
Copy code
pipeline {
  // ...
  
  stages {
    // ...
  }
  
  // ...
  
  post {
    always {
      // Remove the container stack
      sh 'docker rm -f $(docker ps -aq)'
    }
  }
}
The docker rm -f $(docker ps -aq) command removes all running and stopped containers. The -f option forces the removal, and $(docker ps -aq) retrieves the IDs of all containers. Adjust the command based on your specific container management needs.

Save and run the pipeline: Save the pipeline configuration and trigger a build manually or wait for the webhook trigger. The pipeline will execute the defined stages and steps, and after completion, it will remove the container stack.

By following these steps, you'll add a post-build step in the Jenkins pipeline script to remove the container stack using Docker CLI commands. Make sure to adjust the commands to fit your specific needs, such as removing specific containers or using container orchestration tools like Docker Compose, Ansible, Chef, or Puppet for cleanup.















Step 8: Document the Steps and Algorithms:

1.	Create a documentation file: Start by creating a new document or file to serve as the project's documentation. You can use a word processor like Microsoft Word or a markdown editor like Visual Studio Code with appropriate extensions.
2.	Project and tester details: Begin the documentation by providing information about the project, such as its name, description, and any relevant details. Include the name and contact information of the tester or testing team involved in the project.
3.	Concepts used in the project: Explain the concepts used in the project, such as Jenkins, Docker, Git, and AWS. Provide a brief overview of each concept, its purpose, and its relevance to the project. Include any necessary definitions and explanations to ensure the reader understands the context.
4.	Step-by-step setup and configuration process: Document each step of the project's setup and configuration process. Include clear instructions, explanations, and screenshots if applicable. Cover the following aspects:
•	Setting up the Jenkins architecture on an AWS instance: Explain how to provision an EC2 instance, install and configure Jenkins on it, and ensure it meets the necessary system requirements.
•	Installing necessary plugins: Describe how to install and configure the required Jenkins plugins to enable build creation on a containerized platform.
•	Creating and running the Docker image: Document the steps to define a Dockerfile, build and tag the Docker image using the Jenkins pipeline script, and execute the build process.
•	Executing automated tests: Explain how to incorporate automated tests into the Jenkins pipeline script, including the necessary steps and configurations to run the tests on the created build.
•	Setting up Git repository and webhook: Provide instructions on how to create a Git repository, configure repository settings, generate SSH keys on the Jenkins server, and set up a webhook to trigger Jenkins jobs.
•	Building and pushing Docker image: Describe the process of building and tagging the Docker image, as well as pushing it to a private repository. Include details on configuring Docker credentials and registry settings.
•	Exposing the application on respective ports: Explain how to deploy the Docker image on a container and configure the container to expose the necessary ports for accessing the application.
•	Removing the container stack: Document the steps to remove the container stack after completing the job, using Docker CLI commands or container orchestration tools.
5.	Algorithms used for computation: If there are any algorithms used for computation or any other relevant algorithms in the project, provide detailed explanations and descriptions of those algorithms. Include the underlying logic and any specific considerations.
6.	Test cases and results: Document the test cases, their execution steps, and the recorded results. Include details on the test scenarios, input data, expected outcomes, and any specific configurations required to run the tests successfully. Provide screenshots or logs to support the recorded results.
7.	Jenkins pipeline script and supporting files: Include the complete Jenkins pipeline script (Jenkinsfile) in the documentation. If there are any supporting scripts, configurations, or files used in the project, provide them as well. This helps the reader understand the implementation details and reproduce the project if needed.
8.	GitHub repository link: Include a link to the GitHub repository where the project code is hosted. This allows the reader to verify the project's completion and access the source code for further reference.
9.	Conclusion and USPs: Conclude the documentation by discussing potential enhancements to the application. Highlight any unique selling points (USPs) that make the application stand out from others. This could include features, performance improvements, scalability, or any other advantages offered by the application.
By following these steps, you'll create comprehensive documentation that covers the setup and configuration process, algorithms used, test cases and results, Jenkins pipeline script, project

