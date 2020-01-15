# TouchBistro
TouchBistro SRE challenge

This project was created to solve the "TouchBistro SRE Challenge"

The requirements for the project is listed Bellow

    EC2
    ● Create an ec2 instance in your own AWS account.

    ● Create the instance using Infrastructure as Code tool of your choice (ie Cloudformation,Terraform, or an AWS SDK.)

    ● Configure the ec2 instance to be a basic nginx server (just install nginx the basic configuration is fine), this will display the basic nginx web page.

    ● Make use of configuration management for installing the nginx server. (Chef, Ansible,Puppet)

    ● Create an internal Route 53 domain and A records, this should also be done via Infrastructure as Code.
    
    ● Create ec2 instance in a public zone so that it can be accessed over 443 from the internet.

Standard:
    1. Create a github repo for this project. A README file fully documenting the project is required. Versioning and branches are at your discretion.
    2. Be sure to store keys and credentials (if necessary) safely via a mechanism of your choice. (Online services such as google drive, or a password manager is fine).
    3. Please create your own AWS account for this project and create a user for us to have access. (Read only access is fine)

Bonus:
    1. Add a Let’s Encrypt certificate to the nginx configuration using config management.
    2. Ensure that access to SSH to the EC2 is blocked from the world but allow HTTPS traffic.

##How the project was solved
The process of the executing this challenge is listed below:
1. Inorder to provision an EC2 instance on AWS using cloudformation, a template named "aws-cf-ec2.json" was created. The following resources were created using that Template :
   a. An EC2 Instance in a VPC and custom subnet.
   b. A security group that was attached to the EC2 instance to premit only HTTPS traffice into the instance.
   c. A pivate hosted zone in Route 53 for internal routing.
   d. A Recordset within the private zone for internal routing using the private IP address

2. Installation of the nginx server was done using an Ansible playbook, this playbook is in the Ansible-Config/templates folder, this playbook installs nginx and ensure the service is up and running.
3. After installing the nginx server, a playbook was used by ansible to automatically create a Let's Encrypt certificate and install it in the nginx configuration to enable secure connection to the webserver. The name of the playbook that was used is in the Ansible-Config/ folder and it is named playbook.yml, it also references some other files in the templates folder namely: nginx-http.j2, nginx-le.j2,nginx.conf.j2
4. 