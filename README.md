# Deploy-application-firewall
Deploy an application behind Firewall on Azure

## Objective


### Skills Learned



### Tools Used



## Steps

- Step 1: Create Resource group
  
- Step 2: Create VNET (virtual network)
  - Step 2.1 Choose the resource group we created and click on next.
  - Step 2.2 In the security tab enable Azure Bastion, click on create a public ip-adress and we'll choose standard
  - Step 2.3 Click on enable Azure Firewall, choose the option basic. Also click on policy and create a new policy.
  - Step 2.4 In IP adresses change the CIDR range to a desired amount, we'll just keep at /16 for now. click on review + create and create
- Step 3 Create VM (Virtual Machine)
  - Step 3.1 Choose the resource group we created, and give the VM an appropriate name.
  - Step 3.2 Choose a region that is close.
  - Step 3.3 In image select Ubuntu server 20.04, and in VM architecture we'll keep x64
  - Step 3.4 In authentication select SSH public key, and generate a new key pair.
  - Step 3.5 In networking select the default subnet, also scroll down and enable delete NIC when VM is deleted. click on review + create and create. Remember to download the private key.
 - Step 4 Use Bastion to connect to the VM
   - Step 4.1 in the VM we created click on Bastion.
   - Step 4.2 In Authentication type select SSH private key from local file, in username provide a username and in the local file select the private key we downloaded.
   - Step 4.3 Now click on connect and we'll be connected to the VM.
- Step 5 Install and launch Nginx
  - Step 5.1 in the vm we just opened run sudo su to gain root privileges
  - Step 5.2 Run apt-get update to update the packages on the machine
  - Step 5.3 Run apt-get install nginx-y to install nginx
  - Step 5.4 Run cd /var/www/html this command will transfer us to the html folder where we can upload the script we want nginx to run
  - Step 5.5 We'll create a new html file by running nano index.html and just writing simple html code inside.
  - Step 5.6 run systemctl stop nginx followed by systemctl start nginx to include the file we wrote.
- Step 6 Configure firewall to access the page hosted on the nginx server.
  - Step 6.1 go back to the Azure platform and find the firewall we created in step 2, click on the firewall policy to modify it
  - Step 6.2 Under settings click on DNAT rules and add a rule collection
  - Step 6.2 Fill in a value for name, in priority we'll set the number to 100 as this will take the highest priority
  - Step 6.3 Now click on add rule and select the rule we created
  - Step 6.4 In source IP we'll fill in the ip of our host machine to allow access, if we want multiple people to have access we can also add their ip here.
  - Step 6.5 In destination IP we'll fill in the public ip adress of the firewall
  - Step 6.6 In destination ports we'll fill in any number to use as the firewall port
  - Step 6.7 In translated type we'll select IP address and in translated address we'll fill in the private ip address of our VM
  - Step 6.8 In translated port we'll fill in 80 as Nginx runs on port 80, now we'll click on save

 ## Now we can access the Nginx application from the browser on our host machine
