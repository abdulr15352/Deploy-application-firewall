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
