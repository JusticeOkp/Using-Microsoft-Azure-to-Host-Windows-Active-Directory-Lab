# Using-Microsoft-Azure-to-Host-Windows-Active-Directory-Lab
This is a 10-step walkthrough on how to set up an Active Directory lab within the Microsoft Azure cloud service. This Active Directory scenario will contain two domain controllers on the same domain to simulate a real-life scenario. 

## Prerequisites
- Microsoft Azure
- Windows Remote Desktop Connection
## Steps 
1. Create Resource Group:
- Once logged in, redirect to the Home portal for Microsoft Azure. Under 'Azure services' select
'Resources Groups'. Select 'Create a resource'.
- Name Resource Group 'ADLAB'.
2. Virtual Network:
- Under Basics Tab: Name the Virtual Network 'OnSite'.
- IP Addresses Tab: Click on the blue highlighted 'default' under the Subnet name. Change Subnet
Address range to '10.0.1.0/24'.
- Security Tab: Under Security make sure all tabs are Disabled. Skip the 'Tags' tab. Review & Create and allow the Virtual Network to deploy.
![image](https://github.com/user-attachments/assets/c0f5b3a3-8a8a-4899-be61-7e6b4793995f)
3. Create First Domain Controller VM:
- Redirect to the Home portal for Microsoft Azure. Under 'Azure services' select 'Virtual Machines'
then select 'Create'.
- Add to Resource Group 'ADLAB'. Name the virtual machine 'DC1'.
- Change Availability option to Availability set and Create an availability set named 'ADLAB'.
- Use a Windows Server 2019 Database server image (2vCPUs, 8GB memory). Create username and password.
- Disks Tab: Change OS Disk type to Standard HDD. Create and attach a 10GB disk for Active Directory installation.
- Networking Tab: Ensure the virtual network is 'OnSite' and RDP 3389 is open. Management tab: Disable boot diagnostics. Review & Create.
4. Configuring Virtual Machine 1:
- Connect to 'DC1' using RDP. Format the F: Disk drive for Active Directory.
- Within Server Manager, add Active Directory Domain Services via 'Manage > Add Roles and Features'.
- Promote the server to a domain controller and add a new forest named 'myazurelab.com'.
- Use the F: drive for Active Directory data paths. Restart the VM after installation.
![image](https://github.com/user-attachments/assets/59c02b16-d953-48b1-8fb6-17e112150d16)
5. Create Second Domain Controller VM:
- Similar to Step 3, create a second VM named 'DC2' in the same Resource Group and Availability Set.
- Attach a 10GB disk for Active Directory. Ensure the network settings are identical to 'DC1'.
- Deploy the VM and configure it to join the domain 'myazurelab.com'.
6. Configuring DNS on DC1:
- Within 'DC1', set the IP to Static via Networking > IP Configurations.
- Copy the Static IP of DC1.
- In the 'OnSite' Virtual Network, set Custom DNS servers and paste the IP for DC1. Restart DC1 and DC2.
7. Adding DC2 to the Domain:
- RDP into DC2. Format the F: Disk drive for Active Directory.
- Within Server Manager, join DC2 to the domain 'myazurelab.com' via 'Local Server > Workgroup > Domain'.
- Use DC1 credentials to authorize DC2. Restart DC2.
- Add Active Directory Domain Services on DC2 and promote it to a domain controller replicating from DC1.
8. Configuring DNS on DC2:
- Within DC2, set the IP to Static and copy the address.
- In 'OnSite' Virtual Network, add DC2's IP as a secondary DNS server. Restart DC1 and DC2 to update DNS.
9. Configuring Subnets in Active Directory:
- Copy IPv4 subnets from the 'OnSite' Virtual Network.
- Within DC1, use 'Active Directory Sites and Services' to add the subnets and assign them to the 'OnSite' site.
10. Confirming Dual Domain Controllers:
- On DC1, create a test user in 'Active Directory Users and Computers'.
- Switch to DC2 and confirm replication by locating the test user in the same directory.
- Dual domain controllers are now functional.
