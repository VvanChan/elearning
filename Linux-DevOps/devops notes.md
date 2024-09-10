# I Introduction to DevOps and Site Reliability Engineering

## Table of Contents

- [I Introduction to DevOps and Site Reliability Engineering](#i-introduction-to-devops-and-site-reliability-engineering)
  - [Table of Contents](#table-of-contents)
  - [1 Introduction to DevOps/SRE](#1-introduction-to-devopssre)
  - [2 Major Cloud Services](#2-major-cloud-services)
  - [3 Containers and Kubernetes](#3-containers-and-kubernetes)
    - [3.1 Containers vs. Virtual Machines](#31-containers-vs-virtual-machines)
      - [Virtual Machines: Isolated and Versatile](#virtual-machines-isolated-and-versatile)
      - [Containers: Lightweight and Efficient](#containers-lightweight-and-efficient)
          - [Key characteristics](#key-characteristics)
        - [Container Orchestration](#container-orchestration)
      - [Which One to Choose?](#which-one-to-choose)
    - [3.2 Docker](#32-docker)
      - [Components](#components)
    - [3.3 Kubernetes](#33-kubernetes)
      - [Functionalities](#functionalities)
      - [Components](#components-1)
    - [3.4 lab set up `Docker`, `Podman` and `Kubernetes`](#34-lab-set-up-docker-podman-and-kubernetes)
      - [3.4.1 Prerequisites](#341-prerequisites)
      - [3.4.2 Docker: Install](#342-docker-install)
      - [3.4.3 Docker: Container lifecycle](#343-docker-container-lifecycle)
      - [3.4.4 Docker: Image](#344-docker-image)
      - [3.4.5 Docker: Uninstall](#345-docker-uninstall)
      - [3.4.6 Podman: Install](#346-podman-install)
      - [3.4.7 K8s: Set up Kubernetes Cluster with kind](#347-k8s-set-up-kubernetes-cluster-with-kind)
      - [3.4.8 K8s: Deploy Sample Application](#348-k8s-deploy-sample-application)
      - [3.4.9 troubleshooting: `cgroup` set instead of `cgroup2`](#349-troubleshooting-cgroup-set-instead-of-cgroup2)
  - [4 Infrastructure as Code (IaC)](#4-infrastructure-as-code-iac)
    - [Fundamental features](#fundamental-features)
    - [Tools](#tools)
    - [Terraform](#terraform)
    - [OpenTofu](#opentofu)
    - [AWS CloudFormation](#aws-cloudformation)
    - [lab 5 set up `Terraform` and `OpenTofu`](#lab-5-set-up-terraform-and-opentofu)
      - [Install Terraform](#install-terraform)
      - [Set up the GCP Service Account and Configure Google Cloud SDK](#set-up-the-gcp-service-account-and-configure-google-cloud-sdk)
      - [Create a **new** Google Cloud Compute Engine Instance Using Terraform](#create-a-new-google-cloud-compute-engine-instance-using-terraform)
      - [Set up OpenTofu](#set-up-opentofu)
  - [5 software development practice: Continuous Integration/Continuous Delivery (CI/CD)](#5-software-development-practice-continuous-integrationcontinuous-delivery-cicd)
    - [lab 6 set up `Jenkins`](#lab-6-set-up-jenkins)
  - [6 Observability](#6-observability)
    - [lab 7 Set up `Prometheus` and `Grafana`](#lab-7-set-up-prometheus-and-grafana)
  - [7 Site Reliability Engineering (SRE)](#7-site-reliability-engineering-sre)


## 1 Introduction to DevOps/SRE
DevOps combines development and operations practices to improve collaboration, automate processes, and speed up software delivery. It aims for continuous improvement and reliable releases. 

Site Reliability Engineering (SRE), developed at Google, applies software engineering principles to enhance the scalability and reliability of infrastructure and operations.

## 2 Major Cloud Services
* **Compute Services** provide the power to run applications and process data in the cloud. Virtual Machines (VMs) offer versatile computing environments, while containers managed by tools like Google Kubernetes Engine (GKE) or Azure Kubernetes Service (AKS) offer lightweight, scalable application deployment.
* **Storage Services** act as the cloud's memory. Object storage (e.g., Amazon S3) is a vast digital warehouse for all types of data, block storage (e.g., Google Cloud Persistent Disks) provides traditional storage options, and file storage services (e.g., AWS EFS) organize data in a file structure for easy access.
* **Database Services** manage structured data. Relational databases (e.g., AWS RDS, Azure SQL Database) provide scalable solutions, while NoSQL databases (e.g., MongoDB Atlas) offer flexibility for unstructured or semi-structured data.
* **Networking Services** connect cloud components. Virtual networks (e.g., AWS VPC) provide digital infrastructure, while Content Delivery Networks (CDNs) like Cloudflare speed up content delivery by caching it closer to users.
* **Security and Identity Services** protect data and resources. Identity and Access Management (IAM) services control access, while encryption services (e.g., AWS KMS) safeguard sensitive information.
* **Machine Learning and AI Services** offer tools to add intelligence to applications. Platforms like Google AI Platform provide machine learning capabilities, and Natural Language Processing (NLP) services (e.g., AWS Comprehend) analyze human language.
* **Serverless Computing** lets developers focus on code without managing servers. Function as a Service (FaaS) offerings like AWS Lambda run code in response to events.
* **IoT Services** manage and connect Internet of Things (IoT) devices. Platforms like AWS IoT facilitate device management, and edge computing services (e.g., Google Cloud IoT Edge) process data closer to its source.
* **DevOps and CI/CD Services** streamline software development. Continuous Integration tools (e.g., Jenkins) automate testing and integration, while container orchestration tools (e.g., Kubernetes) manage and deploy containerized applications.
* **Analytics and Big Data Services** handle large datasets. Platforms like AWS EMR process and analyze data, while data warehousing services (e.g., Google BigQuery) offer scalable solutions for querying massive data volumes.

## 3 Containers and Kubernetes
### 3.1 Containers vs. Virtual Machines 
Containers and virtual machines (VMs) are both virtualization technologies, but they serve different purposes and offer distinct advantages. Understanding these differences can help determine which technology is best suited for your needs.

#### Virtual Machines: Isolated and Versatile
Virtual machines provide a fully isolated environment with its own operating system (OS) and dependencies. They run on a hypervisor that <span style="color:blue">abstracts physical hardware</span>, allowing multiple VMs on a single machine. Key characteristics include:

- **Strong Isolation**: Each VM is an independent entity with its own resources and OS, enhancing security by preventing vulnerabilities in one VM from affecting others.
- **Compatibility**: VMs can run various operating systems on the same hardware, making them versatile for running legacy applications or different OS versions.
- **Complete Abstraction**: VMs provide full hardware-level virtualization, operating as if on dedicated physical machines.
- **Resource Overhead**: VMs require more resources due to separate OS instances, leading to higher costs and slower startup times compared to containers.

#### Containers: Lightweight and Efficient
Containers package and run <span style="color:blue">applications</span> with their dependencies, isolated from the host system. They offer a lightweight and efficient alternative to VMs. 

###### Key characteristics
- **Resource Efficiency**: Containers share the host OS kernel, reducing overhead and freeing up resources. They are more resource-efficient than VMs.
- **Rapid Startup and Scalability**: Containers can start and stop almost instantaneously, making them highly scalable and suitable for handling increased workloads.
- **Isolation and Security**: Containers are isolated from each other and the host, but sharing the OS kernel means vulnerabilities could affect all containers on the host.
- **Ease of Management**: Containers are lightweight and easy to manage, making them ideal for applications needing frequent updates and CI/CD workflows.

##### Container Orchestration
- Benefits
  - Automating container management
  - Streamlining complex workflows
  - Improving resource utilization
  - Enforcing consistency and control
  - Enabling multi-cloud deployments
- Examples
  - Kubernetes
  - Docker Swarm
  - Apache Mesos
  - Nomad

#### Which One to Choose?
- **Development and Testing**: <span style="color:orange">Containers</span> are favored for their agility and ease of management, providing reproducible environments and simplifying deployment.
- **Microservices Architecture**: <span style="color:orange">Containers</span> suit microservices-based architectures, enabling easy scaling and deployment of independent services.
- **Legacy Applications**: <span style="color:orange">VMs</span> are better for running legacy applications that may not fit modern container environments, allowing for the maintenance of existing infrastructure.
- **High Isolation Requirements**: <span style="color:orange">VMs</span> offer stronger isolation and security, making them suitable for industries like finance and healthcare where data privacy is crucial.

### 3.2 Docker
Docker is a platform that uses containerization technology to streamline the <span style="color:blue">development, deployment, and scaling of applications</span>. 

#### Components
- **Docker Engine**: The core component of Docker, managing the container lifecycle and enabling the creation, shipment, and running of applications in containers.
  - Containerd
  - runC
  - libcontainer
- **Docker Client**: A <span style="color:blue">command-line tool</span> for interacting with the Docker daemon (Docker Engine), allowing users to build, run, and manage containers.
- **Docker Images**: Standalone, executable <span style="color:blue">packages</span> containing an application and its dependencies, built from Dockerfile instructions and stored in registries.
- **Docker Registry**: A <span style="color:blue">repository</span> for storing and distributing Docker images, with Docker Hub as the default public registry and options for private registries.
- **Docker Compose**: A tool for defining and running <span style="color:blue">multi-container</span> applications using a `docker-compose.yml` file, simplifying the management of complex applications.
- **Docker Swarm**: Docker’s native <span style="color:blue">clustering and orchestration</span> tool, enabling the creation and management of a swarm of Docker nodes for deploying and scaling services across containers.
- **Networking and Storage Drivers**: Components that facilitate container <span style="color:blue">communication and storage management</span>, customizable based on application needs.

### 3.3 Kubernetes
Kubernetes (k8s) is an open-source platform that <span style="color:blue">orchestrates and automates</span> the deployment, scaling, and management of containerized applications</span>. Kubernetes is designed for complex, distributed systems that require scalability, resilience, and flexibility.

#### Functionalities
- Deployment: Automates container deployment across nodes.
- Scaling: Adjusts application size based on demand.
- Load Balancing: Distributes traffic evenly across instances.
- Service Discovery: Enables applications to find and communicate with each other.
- Health Monitoring: Monitors and restarts failing applications.
- Security: Manages access control and application protection.
- Self-healing: Restarts and replaces failed containers, ensuring they are ready before advertising.

#### Components
- <span style="color:red">Control Plane Node</span>
  - API Server: Handles requests, validates, and updates cluster objects via the Kubernetes API.
  - Cluster Data Store (etcd): Persistently stores the cluster's desired and current state.
  - Controller Manager: Manages controllers to regulate cluster state, including node management and replication.
  - Scheduler: Assigns pods to nodes based on resource availability and policies.
- <span style="color:red">Worker Node</span>
  - Kubelet: Communicates with the control plane to ensure containers are running as expected and reports node status.
  - Container Runtime: Manages the container lifecycle (e.g., containerd).
  - Kube Proxy: Handles local networking, routing, and load balancing within the node, and updates network rules.
- Core concepts
  - Pod: The smallest deployable unit, representing a single process with shared network and storage resources.
  - ReplicaSet: Ensures a specified number of pod replicas are running and can scale as needed.
  - Deployment: Manages updates to applications, facilitating rollouts and rollbacks.
  - Service: Defines access policies for a set of pods and abstracts network details.
  - Volume: Provides persistent storage for containers.
  - Namespace: Organizes and isolates resources within the cluster.
  - ConfigMap and Secret: Manage configuration data and sensitive information separately from application code.
  - Kubernetes Dashboard: A web-based UI for managing and monitoring the cluster.

### 3.4 lab set up `Docker`, `Podman` and `Kubernetes`
#### 3.4.1 Prerequisites
Ubuntu 20.04 host (previous lab)
1. Google Cloud > Compute Engine > Virtual machines > VM instances > CREATE INSTANCE
2. Boot disk > Image: Version = Ubuntu 20.04 LTS
3. SSH Key: local public key

#### 3.4.2 Docker: Install 
``` bash
#1. Install Docker:
curl -fsSL https://get.docker.com/ | sh
#2. Enable Docker to start on boot:
sudo systemctl enable --now docker
#3. Check that the Docker service is running: 
sudo systemctl status docker
#4. Add current user to the docker group: 
sudo usermod -aG docker $USER
#5. Refresh shell session (by exiting & logging in again). 
#6. Verify the Docker installation -3.4.6.3
docker ps
```
#### 3.4.3 Docker: Container lifecycle  
```bash
#1.  pulls the hello-world image from Docker Hub and runs it as a container. 
sudo docker container run hello-world
#2. List the container running on our host
sudo docker container ls
#3. List all containers
sudo docker container ls -a
#4. starts up an ubuntu container and opens an interactive bash shell in the foreground:
sudo docker container run -it ubuntu /bin/bash
#5. Start an ubuntu container, detach: run in background; interactive; tty: Allocate a pseudo-TTY
sudo docker container run -dit ubuntu /bin/bash
#6. interact with the background container by attaching to the container.
sudo docker container attach <id returned in step 5>
     root@xxx:/#
     #Note: press ctrl+p,ctrl+q to send the container to background.
     root@xxx:/# read escape sequence
#7. start an nginx web server and expose a port on the host to access the application. 
sudo docker container run -dit --name webserver -p 8080:80 nginx
#8. access the application via port 8080 of the localhost
curl localhost:8080
#9. Create a custom html page and copy it to our container.
cat > index.html << EOF
    > Welcome to Docker Training!!
    > EOF
sudo docker container cp index.html webserver:/usr/share/nginx/html/index.html
#10. test access
curl localhost:8080
#11.  Stop and remove the container.
    sudo docker container stop webserver
#12. Remove all the stopped and completed containers.
    sudo docker container prune
```

#### 3.4.4 Docker: Image 
```bash
#1. List the available images on our host
sudo docker image ls
#2. pull an image from the registry
sudo docker image pull alpine
#3. override the default fetching of the image with tag as latest by specifying 2.0
sudo docker image pull gcr.io/google-samples/hello-app:2.0
#4. build a simple image using Dockerfile -3.4.6.6
cat > Dockerfile <<EOF
    > FROM nginx
    > COPY . /usr/share/nginx/html
    > EXPOSE 80/tcp
    > CMD ["nginx", "-g daemon off;"]
    > EOF
sudo docker image build . -t nginx:version1 #name:tag
#5. Create a container using the image we built and access the application.
sudo docker container run -dit --name webapp -p 8081:80 nginx:version1
curl localhost:8081
#6. Clean up all unused images on the host
sudo docker image prune -a
```
#### 3.4.5 Docker: Uninstall
```bash
#1. Uninstall the Docker Engine, CLI, containerd, and Docker Compose packages:
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
#2. Delete Images, containers, volumes, or custom configuration files manually
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```
#### 3.4.6 Podman: Install
```bash
#1. download Podman binaries
curl -fsSL -o podman-linux-amd64.tar.gz > https://github.com/mgoltzsche/podman-static/releases/latest/download/podman-linux-amd64.tar.gz
tar -xf podman-linux-amd64.tar.gz
sudo cp -r podman-linux-amd64/usr podman-linux-amd64/etc /
#2. Verify the Podman installation:
podman --version
#3. List the existing container -3.4.2.6
sudo podman container ps
#4. Create a container
sudo podman container run hello-world
# pick docker.io/library/hello-world:latest
#5. list the available images on our host
sudo podman image ls
#6. build an image using the previous Dockerfile -3.4.4.4
sudo podman image build . -t nginx:podman
```
#### 3.4.7 K8s: Set up Kubernetes Cluster with kind 
- Prerequisite: install Docker, `sudo systemctl stop podman`
- `kind` (Kubernetes IN Docker): for creating Kubernetes clusters locally using Docker containers.
- `kubectl`: CLI for interacting with, deploying apps and managing resources in Kubernetes clusters.
```bash
#1. Download the binary
curl -sSL -O "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
#2. Modify permissions
chmod +x kubectl
#3. Move to /usr/local/bin:
sudo mv kubectl /usr/local/bin
#4. install kind
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64 #AMD64 / x86_64
[ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-arm64 #ARM64
#5. Modify permissions
chmod +x ./kind
#6. Move the kind binary /usr/local/bin
sudo mv ./kind /usr/local/bin/kind
#7.  create a simple cluster with default configuration. A config file with additional configurations can be created and passed to the create cluster command during the run time. To keep it simple, we are going to make use of default configurations.
kind create cluster
#8. Verify the cluster by checking how many namespaces we have on our cluster
kubectl get ns
```
#### 3.4.8 K8s: Deploy Sample Application
```bash
#1. Create webapp.yaml file with contents below. This file defines a Kubernetes deployment for your Sample Application.
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp
spec:
  replicas: 2
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
      - image: nginx
        name: nginx
#2. Deploy the application:
kubectl apply -f webapp.yaml
#3. Verify the deployment: 
kubectl get deployments
#4. Set up port forwarding to access the application. run this in a new terminal tab to keep the port forwarding active throughout your testing session.
kubectl port-forward deploy/webapp 8080:80
#5. Test the sample application by sending an HTTP request. 
curl http://localhost:8080/
#6. Cleanup by deleting the deployment: 
kubectl delete deploy webapp
```
#### 3.4.9 troubleshooting: `cgroup` set instead of `cgroup2`
```bash
#1. Edit the GRUB Configuration File
sudo nano /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash systemd.unified_cgroup_hierarchy=1"
#2. update GRUB
sudo update-grub
#3. Reboot to apply the changes
sudo reboot
#4. verify cgroup config. You should see cgroup2 mounted at /sys/fs/cgroup/unified.
mount | grep cgroup
podman info --format '{{.Host.CgroupsVersion}}'
#5. Check Docker and containerd Status
sudo systemctl status docker
sudo systemctl status containerd
```
## 4 Infrastructure as Code (IaC)
### Fundamental features
1. Declarative Configuration
2. Idempotent Operations: same config -> same result, regardless of the initial or intermediate states
3. <span style="color:red">Version Control</span> Integration
4. Dependency Management
5. Parallel Execution

### Tools
Infrastructure as Code (IaC) Tools <span style="color:red">automate</span> the provisioning and management of infrastructure by treating it as code. Here’s a concise overview of the different types:

- **Declarative IaC Tools**: Define the <span style="color:blue">desired state</span> of infrastructure, and the tool ensures it is achieved.
  - **Terraform**: Uses a declarative language to define infrastructure; supports multiple cloud providers.
  - Pulumi: Allows defining infrastructure using programming languages like Python and TypeScript.
  - SaltStack: Uses YAML for IaC; focuses on automation and centralized management.
- **Configuration Management Tools**: Manage <span style="color:blue">software configurations</span> on existing infrastructure, not purely IaC but essential for managing the software layer.
  - Ansible: Uses YAML scripts for configuration management and provisioning.
  - Chef: Ruby-based DSL for configurations; combines declarative and imperative styles.
  - Puppet: Uses Puppet DSL for centralized and controlled infrastructure management.
- **Container Orchestration Tools**: <span style="color:blue">Automate</span> deployment and scaling of containerized applications, often integrating IaC principles.
  - **Kubernetes**: Manages containerized applications; Helm helps define and version Kubernetes configurations.
  - Nomad: Manages various environments (bare metal, VMs, clouds); focuses on simplicity.
  - CloudHedge: Offers container-native management with virtual machines and serverless functions.
- **Cloud Native IaC Tools**: Designed for <span style="color:blue">specific cloud</span> environments with native integrations.
  - Azure Resource Manager (ARM) Templates: JSON files for automating Azure resource deployment.
  - AWS CloudFormation: JSON or YAML templates for defining and managing AWS resources.

- Considerations for Choosing IaC Tools: Compatibility, Community Support, Scalability, Ease of Learning, Security

### Terraform
- open-source, simplifies setup and management by defining infrastructure in human-readable configuration files, across multiple cloud providers like AWS, Azure, GCP, and on-premises data centers.
- benefits: modularized, consistent, <span style="color:red">reusable</span>, <span style="color:red">scalable</span>
- **Key Features**
  - Multi-Cloud Support: Provisions and manages infrastructure across multiple cloud providers (AWS, Azure, GCP) and on-premises.
  - Automation: Automates infrastructure changes, reducing manual errors and configuration drift.
  - Version Control: Allows infrastructure to be managed like code, improving collaboration and enabling rollbacks.
  - Modularity: Supports reusable infrastructure components ("Terraform modules") for consistency and <span style="color:red">efficiency</span>.

### OpenTofu
- open source, forked from Terraform version 1.5, address the limitations of the BSL
- benefits: community-driven, Flexibility and control, Future-proof, Continuous development and improvement (robust integration capabilities), *Familiarity for Terraform users*, user-friendly interfaces
- benefits for developers and DevOps engineers: Increased agility and productivity, Enhanced security and compliance, *Reduced vendor lock-in*
- special features (vs CloudFormation): parallel execution, custom resource providers.

### AWS CloudFormation
- benefits: Improved consistency and reliability, Increased agility and speed, Reduced costs, Enhanced traceability and auditability
- Key concepts
  - **Templates**: core building blocks. in JSON or YAML and define the resources you want to create and configure in your AWS account.
  - **Stacks**: collections of AWS resources that are created and managed together using a CloudFormation template.
  - Resources: building blocks of the cloud infrastructure, such as Amazon EC2 instances, Amazon S3 buckets, and AWS Lambda functions.
  - Parameters: pass values into templates at the time of stack creation or update.
  - Outputs: retrieve information about the resources created in your CloudFormation stacks.
- benefits for developers and DevOps engineers: Improved code reusability, Simplified infrastructure management, Reduced risk of configuration errors, Faster infrastructure deployment

### lab 5 set up `Terraform` and `OpenTofu`
#### Install Terraform
```bash
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
terraform --version
```

#### Set up the GCP Service Account and Configure Google Cloud SDK
1. install gcloud CLI
```bash
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates gnupg curl
# Import the Google Cloud public key:
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg
# Add the gcloud CLI distribution URI as a package source:
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
sudo apt-get update && sudo apt-get install google-cloud-cli
gcloud init
```
2. create Service Account and authenticate: upload `serviceaccount.json` via "UPLOAD FILE" on upper right corner, ensure success msg is shown. file is uploaded to `~`
```bash
# alternative, but failed due to incoreect private key
sudo ufw allow 22/tcp 
sudo firewall-cmd --reload
sudo apt-get install firewalld
ssh <username>@<your_local_public_ip>
```
3. verify authentication
```bash
export GOOGLE_APPLICATION_CREDENTIALS="/home/<username>/serviceaccount.json"
gcloud auth list
```

#### Create a **new** Google Cloud Compute Engine Instance Using Terraform
1. Create a Terraform configuration file `main.tf`
```json
provider "google" {
  project = "triple-course-430516-g4"
  region  = "us-central1"
  zone    = "us-central1-c"
}
resource "google_compute_instance" "my_instance" {
  name         = "new-vm-for-docker"
  machine_type = "e2-medium"
  zone         = "us-central1-c"
  boot_disk {
    initialize_params {
      image = "ubuntu-2004-focal-v20240808"
    }
}
  network_interface {
    network = "default"
    access_config {
      // Ephemeral IP
    }
  } 
}
```
2. run
```bash
terraform plan
terraform apply
# verify new instance on the VM Instance page
terraform destroy
```
- troubleshooing `Reason: insufficientPermissions, Message: Insufficient Permission`:
Re-authenticate Terraform with Google Cloud
```bash
gcloud auth application-default login
```

#### Set up OpenTofu
```bash
sudo snap install --classic opentofu
tofu -version
# use the same main.tf as before
cd code/
tofu init
tofu plan
tofu apply
tofu destroy
```

## 5 software development practice: Continuous Integration/Continuous Delivery (CI/CD)
- CI:
  - principles: frequent code integration, automated builds, automated testing procedures, immediate feedback loop
  - practices: version control, CI server etc
- Continuous Delivery: software development practice, extends the automated pipeline to include deployment and release processes - automation up to production
- Continuous Deployment: Automated Production Deployment (all code passed test), High Level of Automation, Immediate User Access - automation up to release
  - practices: feature toggles, rollback mechanisms, monitoring and logging etc
- Release Strategies
  - Rolling Release: Continuous incremental updates without distinct versions, ideal for web apps needing rapid feature delivery.
  - Feature Toggles (Feature Flags): Selectively activate or deactivate features, allowing gradual rollouts and testing.
  - Blue-Green Deployment: Uses production and deployment environments to ensure zero downtime and quick rollback for critical systems.
  - Canary Release: Deploys to a small user group first for real-world testing and risk mitigation.
  - Phased (Staged) Rollout: Gradual release to increasing user groups, ensuring stability through controlled monitoring.
- GitOps: uses Git as the single source of truth for managing infrastructure and application configuration in a declarative way.
  - principles: Declarative configuration, Single source of truth, Pull-based reconciliation
  - tools: Argo CD, Flux CD, Tekton CD, Jenkins X

### lab 6 set up `Jenkins`
1. set up Jenkins server on Docker
```bash
# ensure docker is on
sudo systemctl restart docker
docker run --name myjenkins1 -dit -p 8080:8080 -p 50000:50000 -v jenkins_data:/var/jenkins_home  jenkins/jenkins:lts
docker container ls
# capture the initial admin password for Jenkins
docker logs myjenkins1| grep -B 5 initialAdminPassword
# retrieve the password after creation
docker exec -it myjenkins1 sh
$ cat /var/jenkins_home/secrets/initialAdminPassword
```
```bash
# to remove jenkins to address any unexpected issues
docker stop myjenkins1
docker rm myjenkins1 # shorthand
docker volume rm jenkins_data
```
2. configure Jenkins
   1. update Google Cloud Console > VPC Network > Firewall Rules > Firewall rule > default-allow-http > Protocols and ports, allow 8080 at TCP port
   2. `curl ifconfig.io`, access http://<HostIP>:8080
   3. Choose the “Select Plugins to install” option and click on “none”, then click on Install.
   4. 你已跳过创建admin用户的步骤。要登录请使用用户名：'admin' 及用于访问安装向导的管理员密码。
   5. reset password, create new jobs, and play around!

3. remove container
```bash
docker container stop myjenkins1 # more explicitly by including 'container'
docker container rm myjenkins1
docker container rm `docker container ls -a -q` -f
docker image rm `docker image ls -q` -f
docker volume prune -f
```

## 6 Observability
1. 3 pillars
    1. **logs**: Application Logs, System Logs, Audit Logs
    2. **metrics**: Business Metrics, Application Metrics, System Metrics
    3. **traces**: span, trace context, annotations and metadata; Distributed Tracing, Real User Monitoring (RUM); Synthetic Monitoring
2. Key Components
    1. Data Collection:
        1. Instrumentation: Adds code to capture telemetry data (metrics, logs, traces). Tools: OpenTelemetry (standardizatio), [Prometheus](#lab-7-set-up-prometheus-and-grafana), ELK Stack (Elasticsearch, Logstash, Kibana).
        2. Agents: Lightweight programs that collect data from sources like applications and containers. Tools: Datadog Agent, Fluentd, Telegraf.
    2. Data Storage and Management:
        1. Metric Stores: Databases for time-series data. Tools: Prometheus, InfluxDB.
        2. Log Aggregators: Centralized log storage. Tools: Elasticsearch, Logstash, Graylog.
        3. Trace Storage: Specialized storage for high-volume traces. Tools: Jaeger, Zipkin, Honeycomb.
    3. Data Analysis and Visualization:
        1. Dashboards and Graphs: Visualize data. Tools: [Grafana](#lab-7-set-up-prometheus-and-grafana), Kibana.
        2. Alerting: Notifies about critical events. Tools: Prometheus Alertmanager, PagerDuty.
        3. Analytics: Deep analysis for troubleshooting. Tools: Datadog APM, New Relic APM.
    4. Additional Components:
        1. Service Mesh: Provides features like distributed tracing. Tools: Istio, Linkerd.
        2. Chaos Engineering: Tests system resilience by injecting faults. Tools: Chaos Monkey, Gremlin.
3. Building an Observability Stack: Open Source Solutions, Managed Services, Hybrid Approach
  
### lab 7 Set up `Prometheus` and `Grafana`
1. install [Prometheus](https://github.com/prometheus/prometheus/releases)
   ```bash
   cd /tmp
   curl -LO https://github.com/prometheus/prometheus/releases/download/v2.54.1/prometheus-2.54.1.linux-amd64.tar.gz
   tar xvf prometheus-2.54.1.linux-amd64.tar.gz
   sudo mv prometheus-2.54.1.linux-amd64/prometheus /usr/local/bin/
   sudo mv prometheus-2.54.1.linux-amd64/promtool /usr/local/bin/
   sudo mkdir /etc/prometheus
   sudo mv prometheus-2.54.1.linux-amd64/prometheus.yml /etc/prometheus/prometheus.yml
   ```
2. set up a user
   ```bash
   sudo useradd --no-create-home --shell /bin/false prometheus
   sudo chown prometheus:prometheus /usr/local/bin/prometheus
   sudo chown prometheus:prometheus /usr/local/bin/promtool
   sudo chown -R prometheus:prometheus /etc/prometheus
   ```
3. start as a service
```bash
sudo tee /etc/systemd/system/prometheus.service > /dev/null <<EOF
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file=/etc/prometheus/prometheus.yml \
    --storage.tsdb.path=/etc/prometheus/ \
    --web.listen-address=0.0.0.0:9090

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
sudo systemctl status prometheus # check status
```
verify by connecting to http://<external IP>:9090. same as [lab 6](#lab-6-set-up-jenkins), add 9090 to `default-allow-http` TCP port.
- port issue resolve
  ```bash
  # 1 allow http tcp port 9090 at Google Cloud Console
  # 2 Uncomplicated Firewall rules
  sudo ufw status
  sudo ufw allow 9090/tcp
  # 3 iptables rules
  sudo iptables -L -n
  sudo iptables -A INPUT -p tcp --dport 9090 -j ACCEPT
  # 4 prometheus logs
  sudo journalctl -u prometheus -f
  # 5 verify prometheus listening 9090
  sudo ss -tuln | grep 9090
  # not working yet
  ```
- reinstall Prometheus
  ```bash
  sudo systemctl stop prometheus
  sudo rm -f /usr/local/bin/prometheus
  sudo rm -f /usr/local/bin/promtool

  sudo userdel -r prometheus

  sudo rm -rf /etc/prometheus
  sudo rm -rf /var/lib/prometheus

  sudo rm -f /etc/systemd/system/prometheus.service

  sudo systemctl daemon-reload
  ```
4. install Grafana
   ```bash
   sudo apt-get install -y apt-transport-https software-properties-common wget
   sudo mkdir -p /etc/apt/keyrings/
   wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
   echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
   sudo apt-get -y update
   sudo apt-get install -y grafana
   ```
5. start Grafana
   ```bash
   sudo systemctl start grafana-server
   sudo systemctl enable grafana-server
   sudo systemctl status grafana-server # check status
   ```
6. configure Grafana to use Prometheus
   1. access http://<external IP>:3000. credentials are both `admin`
   2. Connections > Data Sources > Add Data Source, Select "Prometheus" as the type. use http://localhost:9090 for the url and click "Save & Test".

## 7 Site Reliability Engineering (SRE)
1. Primary goal: To ensure maximum system reliability and uptime through engineering and operations practices
2. Service Level Indicators (SLIs): quantify performance and reliability, e.g. latency, availability, error rate
3. Service Level Objectives (SLOs) = 1 - Error Budget：measurable specific targets, balance release velocity with system reliability, e.g. available 99.9% of time
4. Service Level Agreements (SLAs): clear communication to manage client expectations 
5. 7 Principles of SRE
   1. Embracing Risk
   2. SLOs
   3. Simplicity
   4. Toil Automation
   5. Monitoring and Alerting
   6. Capacity Planning: optimal system performance and reliability
   7. Emergency Response and Blameless Postmortems