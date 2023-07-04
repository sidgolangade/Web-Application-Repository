## Step-1: Developer Server | Terraform | 3-Servers

- Create a Developer Server (EC2 RHEL Instance)
- Install Terraform on the Developer Server - https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli
- Create and Navigate to the new directory for the Terraform project - terraform-ec2-instances
- main.tf: terraform init > terraform apply
- Jenkins Server Installed, Ansible Server Installed, Web Server Installed

## Step-2: Configuring Jenkins Server

For Jenkins Administrative Password:
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

## Step-3: Configuring Ansible Server


## Step-4: Configuring Web Server

To Start Tomcat:
sudo /opt/tomcat/bin/startup.sh
