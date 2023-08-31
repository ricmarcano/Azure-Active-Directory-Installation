<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Azure Active Directory: Installation</h1>
This exercise showcases the procedures I followed to install Active Directory via Azure. Utilizing a shared vnet, I will create two VMs in Azure. This specific lab will concentrate on a single VM, designated for Active Directory installation and configured as the domain controller. The second VM will serve as a client, to be integrated in a subsequent Active Directory Configuration lab.



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Installation Steps</h2>

To ensure that the VMs can communicate with each other, we need to set the domain controller VM's IP address to static. By default, VMs with dynamic IP addresses cannot communicate with each other, even if they are on the same virtual network (vnet). If we do not make this change, the client will not be able to join the domain that will be created later. To do this, in the Azure portal, go to the domain controller VM > Networking > Network Interface > IP Configurations > click the assigned Name > switch allocation to Static > save changes. By doing this, we are making sure that the domain controller VM has a static IP address that will be used as a reference when we make configurations. This will allow the VMs to communicate with each other and the client to join the domain.

![Screenshot 2023-08-31 at 10 28 31 AM](https://github.com/ricmarcano/Azure-Active-Directory-Installation/assets/141169092/ee50b9e3-e5d8-449e-9343-2d66abd866f8)

Once the static IP is configured, proceed to log in to the client VM and test the connectivity with the domain controller. By using the command "ping -t" followed by the domain controller's private IP address, any connection issues will result in timeouts. To address this, on the domain controller VM, it's necessary to activate ICMPv4 within the local Windows Firewall. To access the Windows Defender Firewall settings, simply type "wf.msc" in the search bar > navigate to Inbound Rules > enable the rules associated with Core Networking Diagnostics and ICMP Echo Requests. Upon returning to the client VM, you'll notice that the previously encountered ping issues have been resolved.

![image](https://github.com/ricmarcano/Azure-Active-Directory-Installation/assets/141169092/dd3890b5-e8a6-4aed-89e5-8c58054a495a)

![image](https://github.com/ricmarcano/Azure-Active-Directory-Installation/assets/141169092/4c2dbe28-b547-4a22-8dac-de9ec662d9cb)

![image](https://github.com/ricmarcano/Azure-Active-Directory-Installation/assets/141169092/70525eed-6a75-458f-a39d-217f3906523c)

Now, it's time to initiate the installation of Active Directory on the domain controller VM. Begin by launching Server Manager > select "Add Roles and Features" > click "Next." Verify the private IP address assigned to the domain controller VM. Within the Server Roles section opt for "Active Directory Domain Services" > select "Add Features" > "Next" > "Install."

The subsequent step entails elevating the server to the status of a domain controller. In Server Manager, observe a warning icon located in the upper right corner, beneath a flag. Click on this flag > choose "Promote this server to a domain controller" > opt for "Add a new forest" > input the desired domain name; for instance, I'll use "testdomain.com" Proceed by specifying a password for the domain > click "Next" on each successive screen until reaching "Install."

![image](https://github.com/ricmarcano/Azure-Active-Directory-Installation/assets/141169092/b05727eb-c17b-472f-8fa5-a9adb40547fa)

![image](https://github.com/ricmarcano/Azure-Active-Directory-Installation/assets/141169092/bb782ccd-3414-44db-9aed-bb52ae51c83c)

Note: When you log back into the domain controller VM using Remote Desktop Connection, remember to log in within the domain context. Just type the domain path and the user's name, like "mydomain.com\labuser." For my case, it's "testdomain.com\ricardood." Now that Active Directory is installed, you can set up configurations in future labs, and the client VM will be able to join the domain you created.
