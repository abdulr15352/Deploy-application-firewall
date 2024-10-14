# Deploy-application-firewall
Deploy an application behind Firewall on Azure

## Objective
The Deploy-application-firewal project aimed to deploy a secure Nginx web server within Azure's cloud infrastructure. The primary goal was to gain hands-on experience in setting up virtual networks (VNETs), configuring virtual machines (VMs), and managing network security using Azure Firewall and Azure Bastion. This project provided practical knowledge in cloud networking, security configurations, and web server deployment on Azure.

### Skills Learned
- Azure Resource Management: Creating and managing Resource Groups and Virtual Networks.
- Virtual Machine Deployment: Setting up VMs with proper configurations and networking.
- Secure VM Access: Using Azure Bastion and SSH keys for secure connections to VMs.
- Web Server Installation: Installing and configuring Nginx on an Ubuntu server.
- Firewall Configuration: Setting up Azure Firewall and DNAT rules for external access to internal resources.
- Network Security: Understanding Azure networking, subnets, IP addressing, and security best practices.
- Azure Portal Navigation: Gaining proficiency in using the Azure Portal for resource management.


### Tools Used
- Azure Portal: For creating and managing cloud resources like VMs, VNETs, and Firewalls.
- Azure Bastion: For secure, browser-based SSH access to VMs without exposing public IPs.
- SSH: For secure communication with the VM.
- Nginx: As the web server software to host the web application.
- Azure Firewall: For controlling inbound and outbound network traffic with DNAT rules.

## Steps

- Step 1: Create Resource group
  
  ![image](https://github.com/user-attachments/assets/05adc767-0327-4acd-9d15-ca278558b0da)

- Step 2: Create VNET (virtual network)
  - Step 2.1 Choose the resource group we created and click on next.
    
    ![image](https://github.com/user-attachments/assets/3d28d6fa-127d-4847-94a5-386f82a62e11)

  - Step 2.2 In the security tab enable Azure Bastion, click on create a public ip-adress and we'll choose standard
    
    ![image](https://github.com/user-attachments/assets/f89ab9e2-b938-4643-ae5f-73c08ac0b05a)

  - Step 2.3 Click on enable Azure Firewall, choose the option basic. Also click on policy and create a new policy.
    
 ![image](https://github.com/user-attachments/assets/8f22dac0-e052-486d-9de8-550cd211f441)

  - Step 2.4 In IP adresses change the CIDR range to a desired amount, we'll just keep at /16 for now. click on review + create and create
    
 ![image](https://github.com/user-attachments/assets/1a84a05c-966e-4e94-89fa-eab1af166481)

- Step 3 Create VM (Virtual Machine)
  - Step 3.1 Choose the resource group we created, and give the VM an appropriate name.
  - Step 3.2 Choose a region that is close.
  - Step 3.3 In image select Ubuntu server 20.04, and in VM architecture we'll keep x64
  - Step 3.4 In authentication select SSH public key, and generate a new key pair.
  - Step 3.5 In networking select the default subnet, also scroll down and enable delete NIC when VM is deleted. click on review + create and create. Remember to download the private key.
    
    ![image](https://github.com/user-attachments/assets/4a692201-1557-40a5-b059-6f012ac835aa)

 - Step 4 Use Bastion to connect to the VM
   - Step 4.1 in the VM we created click on Bastion.
   - Step 4.2 In Authentication type select SSH private key from local file, in username provide a username and in the local file select the private key we downloaded.
     
     ![image](https://github.com/user-attachments/assets/112332e7-d30d-48f0-890e-b316f7fb8f46)

   - Step 4.3 Now click on connect and we'll be connected to the VM.
     
  ![image](https://github.com/user-attachments/assets/01aa265a-f457-4792-891c-899ae84c3296)

- Step 5 Install and launch Nginx
  - Step 5.1 in the vm we just opened run sudo su to gain root privileges
    
    ![image](https://github.com/user-attachments/assets/5f395c70-15be-4d15-9eb9-1a4db2117de9)

  - Step 5.2 Run apt-get update to update the packages on the machine
    
    ![image](https://github.com/user-attachments/assets/7e024aa1-5d4d-4504-906f-9d22a7cbd355)

  - Step 5.3 Run apt-get install nginx -y to install nginx
    
    ![image](https://github.com/user-attachments/assets/a51470b7-f02f-466d-b6a0-01e949393ebf)

  - Step 5.4 Run cd /var/www/html this command will transfer us to the html folder where we can upload the script we want nginx to run
  - Step 5.5 We'll create a new html file by running nano index.html and just writing simple html code inside.
    
    ![image](https://github.com/user-attachments/assets/20e0041a-a804-4316-930f-0a42d1778ff7)
  
![image](https://github.com/user-attachments/assets/a7fffc5b-0584-4866-a25b-c9d5e3d6fe4f)

  - Step 5.6 run systemctl stop nginx followed by systemctl start nginx to include the file we wrote.
    
    ![image](https://github.com/user-attachments/assets/6a6e64f3-6f35-44d1-9391-46a9f1a13b11)

- Step 6 Configure firewall to access the page hosted on the nginx server.
  - Step 6.1 go back to the Azure platform and find the firewall we created in step 2, click on the firewall policy to modify it
    
    ![image](https://github.com/user-attachments/assets/70a01bc6-2d19-42de-94bb-55f738b5122c)

  - Step 6.2 Under settings click on DNAT rules and add a rule collection
  - Step 6.2 Fill in a value for name, in priority we'll set the number to 100 as this will take the highest priority
  - Step 6.3 Now click on add rule and select the rule we created
  - Step 6.4 In source IP we'll fill in the public ip of our host machine to allow access, if we want multiple people to have access we can also add their ip here.
    
![image](https://github.com/user-attachments/assets/11ce2334-01cc-4c45-9a65-f380e3f32103)

  - Step 6.5 In destination IP we'll fill in the public ip adress of the firewall
  - Step 6.6 In destination ports we'll fill in any number to use as the firewall port
  - Step 6.7 In translated type we'll select IP address and in translated address we'll fill in the private ip address of our VM
  - Step 6.8 In translated port we'll fill in 80 as Nginx runs on port 80, now we'll click on save

![image](https://github.com/user-attachments/assets/acd9a87f-365b-4289-b633-28fb9ac8b6f9)
    

 ## Now we can successfully access the Nginx application from the browser on our host machine
 
 ![image](https://github.com/user-attachments/assets/134f94dc-bf6c-4bc1-9091-a162cc5f9732)

