# Kubernetes Networking Demo Setup 

## Introduction
Goal of the project is to build a fully automated kubernetes cluster on packet.net baremetal cloud provider. 
Terraform is used to deploy and manage Baremetal server. 
Ansible is used for deploying and configuring kubernetes Cluster. Kubernetes deployment is being right now using kubeadm which is not production grade deployment tool. Current deployment architecture has 1 master node and n worker nodes. At present the script works only for Ubuntu OS.

## Features
* Only update terraform.tfvars for Packet Server deployment configuration
* Fully Automated Kubernetes deployment for Packet Host
* Deploys with Flannel SDN

## Demo Steps
1. Setup Local Deployment Environment ( For Windows user, Linux VM with Internet Access )
    * Install SSH and Generate sshkey

      ```
      sudo apt-get install openssh-server
      sudo ssh-keygen -t rsa
      ```
    
    * Check Python is available if not Install
      
      ```
      python --version 
      ```

    * Install Ansible

      ```
      sudo apt-add-repository ppa:ansible/ansible
      sudo apt-get update
      sudo apt-get install ansible
      ansible --version
      ```
   
    * Install Terraform 
      
      ```
      sudo apt-get install unzip
      wget https://releases.hashicorp.com/terraform/0.11.7/terraform_0.11.7_linux_amd64.zip
      unzip terraform_0.11.7_linux_amd64.zip
      sudo mv terraform /usr/local/bin/
      terraform --version
     ```
  
2. Setup Account on app.packet.net Baremetal Cloud
    
    * Setup Account
    * Create Project
    * Create API Key 
    * Upload SSH key to Project   
             
3. Deploy Kubernetes 

    * Update env_rc file to put in api-key and project-id from Packet Account 
       
       ```
       export PACKET_API_TOKEN= <api-key>
       export TF_VAR_auth_token= <api-key>
       export TF_VAR_project_id= <project-id>
       ```
     
    * Invoke the environment variable
      
      ```
      . env_sh
      ```
    
    * Update Cloud-init file to add your ssh-key and password
    
    * Update terraform.tfvars with appropriate tags and values. (file is self-explanatory)
   

    *  Deploy Servers

       ```
       terraform init
       terraform plan
       terraform apply
       ```
    
   *  Run Ansible Playbook for Kubernetes Deployment

      ```
      $ ansible-playbook -i packet-net.py kubedeploy.yml
      ```
 
 And Your Kubernetes Cluster is up and running on Packet Host.
 
