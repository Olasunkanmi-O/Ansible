# Infrastructure Setup with Terraform and Ansible
## Overview

### This document outlines the steps taken to set up infrastructure with Terraform and configure Ansible for management. The setup is in two phases;

### Firstly, using ansible adhoc commands to manage 2 different managed nodes running on different OS

### Secondly, using ansible playbook to install and enable apache on 4 different servers running different OS 

## First section: Ansible Adhoc command
- Using Terraform to create three servers on AWS.
- Configuring one server as the Ansible Control Node using userdata.
- Configuring the other two servers as Ansible Managed Nodes.
- Running some adhoc ansible modules.


1. We used Terraform to automate the deployment of three servers. Started out by creating the required files on terraform, then moved on to create the infrastructure

    ![](/img/01.create-files.png)
    ![](/img/002.provider.png)
    ![](/img/02.infrastructures.png)
    ![](/img/03.infra.png)
2. Next, we created the security groups (this was set to allow all traffic)
![](/img/04.sg.png)
3. We used userdata for the installation of the ansible on the control node 
![](/img/06userdata.png)
![](/img/05servers.png)
4. Next was to initialize terraform using the init command 
![](/img/07.terraform-init.png)
5. Inorder to validate the syntax of our commands, we used terraform validate command 
![](/img/08.validate.png)
6. Then, we checked the plan to be sure all planned infrastucture are good to go 
![](/img/09.plan.png)
7. Terraform apply was used to start up the creation of the infrastructure
![](/img/10.apply.png)
8. After setting up the servers, we logged into the ansible-installed server and changed the hostname to differentiate it from other servers
![](/img/11.login.png)  
![](/img/12.change-name.png)
9. We then created a new keypair to be used to connect to the other servers that will act as the managed nodes
![](/img/13.gen-key.png)
10. We logged into the second server which is running Ubuntu OS, then we chhanged the name of the server to Ubuntu as well to differentiate it 
![](/img/14server1.png)
11. We located the .ssh directory and modified the authorized key inorder to add the newly generated key so that the ansible-server can easily access it securely 
![](/img/15addkey-1.png)
12. Steps 11 and 12 were repeated for the third server which is running RedHat OS.
![](/img/16.change-name.png)
![](/img/16.addkey-2.png)  
13. Back into the ansible-server, we created the ansible folder and the hosts file in the etc directory 
![](/img/17.hostsfile.png)
14. After creating the hosts file, we then defined the hosts using IP address and the user which ansible will use to access each of the managed nodes
![](/img/18definehosts.png)
15. In order to test the connection between the control node and the managed nodes, we used the ping module of ansible 
![](/img/19pingtest.png)
16. We ran some ansible adhoc commands using the shell module
![](/img/20.filesystem.png)
17. Another command to check free space on the managed nodes which was also a shell module
![](/img/21free-memory.png)

## Summary
Using Terraform, we deployed three servers: one as the Ansible control node and two as managed nodes. With Ansible configured on the control node, we verified connectivity to the managed nodes using the ping module, then tried out some commands using shell module.



## Second section: Using Ansible Playbook
- Using Terraform to create three servers on AWS.
- Configuring one server as the Ansible Control Node using userdata.
- Configuring four other servers as Ansible Managed Nodes.
- Writing a playbook to install apache on the four servers.

1. From step 14 above, we updated the hosts file with the IP address and the users of the other two new servers (CentOs and AmazonLinux)
2. The authorized keys of the new servers were also updated with the public key of the key generated in step 9 above.
3. we logged into the ansible server and created a yaml file called playbook 
![](/img2/001.login.png)
![](/img2/01.playbook.png)
4. We ran the command ansible-playbook
![](/img2/02playbook.png)
5. We tested the page using the IP of each of the servers to ascertain that apache has been successfully installed and started on all the servers.
![](/img2/03.RH.png)
![](/img2/04.centos.png)
![](/img2/05.amazon.png)
![](/img2/05.ubuntu.png)

## Summary
We deployed 5 servers using one of them as the control node and the other four as managed nodes. The four were of different Linux distribution. A playbook was written to manage them remotely through ansible and apache webserver was installed on them.