<p align="center">
  <img src="https://github.com/user-attachments/assets/3e7c5a02-1ae5-4b1c-8106-55172f3e581c" alt="azure-vms-svgrepo-com" width="150" height="auto">
  <h1 align="center">Virtual Machine Management on Azure</h1>
</p>

#### *In this tutorial, we will:*
*- Outline the process for setting up virtual machines.*

*- Deploy virtual machines.*

*- Configure availability features for virtual machines, including scaling options.*

#### Tasks:
 1. Deploy Azure virtual machines with zone resilience using the Azure portal.
 2. Manage scaling for compute and storage resources in virtual machines.
 3. Set up and configure Azure Virtual Machine Scale Sets.
 4. Adjust the scaling of Azure Virtual Machine Scale Sets.

## Task 1: Deploy Azure virtual machines with zone resilience using the Azure portal.

In this task, you will create two Azure virtual machines and distribute them across different availability zones using the Azure portal. Availability zones ensure maximum uptime, offering a 99.99% SLA. To meet this requirement, you need to deploy at least two VMs in separate zones.

1.	Search for and select "Virtual machines", on the Virtual machines blade, click "+ Create", and then select in the drop-down "Azure virtual machine".
2.	On the Basics tab, in the Availability zone drop down menu, place a checkmark next to Zone 2. This should select both "Zone 1" and "Zone 2".
3.	On the Basics tab, complete the following settings: Subscription, Resource group, Virtual machine names (Edit names after selecting availability zones), Region, Availability options, Availability zone, Security type, Image, Azure Spot instance, Size (in this case a DS2_V3), Username, Password, Public inbound ports.

<p align="center">
  <img src="https://github.com/user-attachments/assets/2089e15b-d2b1-4ea7-8dd0-e06eab8d7706">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/36df23cf-a1be-4678-afeb-e77abea961bf">
</p>

4.   Click "Next: Disks >", then configure: OS disk type, Delete with VM (this helps save costs, simplifies resource management, and ensures a clean environment by automatically removing unused disks).

<p align="center">
  <img src="https://github.com/user-attachments/assets/9e7eee22-a076-47f2-8f7b-7ab0e229dd24">
</p>

5.  Click "Next: Networking >", take the default values except for: Delete public IP and NIC when the VM is deleted (this helps save costs, enhances security, and keeps your Azure environment clean). Click "Review + Create". After validation completes, click "Create". Note: Notice as the virtual machine deploys the NIC, disk, and public IP address (if configured) are independently created and managed resources. Wait for the deployment to complete, then select "Go to resource".

<p align="center">
  <img src="https://github.com/user-attachments/assets/0bf9762b-eef9-441a-beb9-33fc1d657b83">
</p>

## Task 2: Manage scaling for compute and storage resources in virtual machines.

In this task, you will resize a virtual machine by selecting a different SKU. Azure allows you to adjust VM specifications as needed, increasing or decreasing compute power and memory. This flexibility also applies to storage, where you can enhance disk performance or expand capacity.

1. On the VM1 virtual machine, in the Availability + scale blade, select "Size".
2. Set the virtual machine size to DS1_v2 and click "Resize". When prompted, confirm the change. Note: Resizing is also known as vertical scaling, up or down.

<p align="center">
 <img src="https://github.com/user-attachments/assets/2e37a8e3-453b-4bf8-ba77-a2a91014d844">
</p>

3. In the Settings area, select "Disks".
4.	Under Data disks select "+ Create" and "attach a new disk". Configure the settings and click "Apply" (leave other settings at their default values): Disk name, Storage type (in this case Standard HDD), Size.

<p align="center">
  <img src="https://github.com/user-attachments/assets/910cf399-5b64-4f33-9aeb-e8893dda9de0">
</p>

5.	After the disk has been created, click "Detach" (if necessary, scroll to the right to view the detach icon), and then click "Apply". Note: Detaching removes the disk from the VM but keeps it in storage for later use.
6.	Search for and select "Disks". From the list of disks, select the VM1 disk object. Note: The Overview blade also provides performance and usage information for the disk.
7.	In the Settings blade, select "Size + performance". Set the storage type to Standard SDD, and then click "Save".

<p align="center">
  <img src="https://github.com/user-attachments/assets/31b00845-9db5-4f4f-b45d-efd196c5f8f7">
</p>

8.	Navigate back to the VM1 virtual machine and select "Disks".
9.	In the Data disk section, select "Attach existing disks".
10.	In the Disk name drop-down, select the VM1 disk.

<p align="center">
  <img src="https://github.com/user-attachments/assets/f1fcfc78-9ff8-4c0a-ba25-bdc454d12ade">
</p>

11.	Verify the disk is now different. Select "Apply" to save your changes. Note: You have now created a virtual machine, scaled the SKU and the data disk size. In the next task we use Virtual Machine Scale Sets to automate the scaling process.

## Task 3: Set up and configure Azure Virtual Machine Scale Sets.

In this task, you will set up an Azure virtual machine scale set spanning multiple availability zones. VM Scale Sets streamline automation by letting you define scaling rules based on metrics, enabling automatic expansion or reduction of resources.

1. In the Azure portal, search for and select "Virtual machine scale sets" and, on the Virtual machine scale sets blade, click "+ Create".
2. On the Basics tab of the Create a virtual machine scale set blade, configure the following settings: Subscription, Resource group, Virtual machine scale set name, Region, Availability zone, Orchestration mode, Security type, Scaling options, Image, Size, Username, Password. Then, go to Networking.
 
<p align="center">
 <img src="https://github.com/user-attachments/assets/9ba5f87e-06f1-41d9-8e58-7298bad1ede8">
</p>

 3. On the Networking page, click the "Edit virtual network" link below the Virtual network textbox and create a new virtual network, when finished, select "OK": Network Name, Address range	10.82.0.0/20, Subnetwork name, Subnet range	10.82.0.0/24.

<p align="center">
<img src="https://github.com/user-attachments/assets/07655f4c-3c00-4960-ba6a-cfba3914ed43">
</p>

<p align="center">
 <img src="https://github.com/user-attachments/assets/abe24ea9-a842-4445-96cb-c6e12774f218">
</p>
 
4. In the Networking tab, click the "Edit network interface" icon to the right of the network interface entry.
5. For NIC network security group section, select "Advanced" and then click "Create new" under the Configure network security group drop-down list.

 <p align="center">
 <img src="https://github.com/user-attachments/assets/7b68c08e-aad1-4702-82dd-0563f03e8120">
</p>

6. On the Create network security group blade, specify the name (leave others with their default values).
7. Click "Add an inbound rule" and configure: Source (Any), Source port ranges (*), Destination (Any), Service (HTTP), Action (Allow), Priority (1010), Name (allow-http), then save the rule.

<p align="center">
<img src="https://github.com/user-attachments/assets/97806a72-e444-4796-98d6-fae8be17016c">
</p>
 
8. Click "Add" and, back on the Create network security group blade, click "OK".
9. In the Edit network interface blade, in the Public IP address section, click "Enabled" and click "OK".
10. In the Networking tab, under the Load balancing section, configure: Load balancing options (Azure load balancer), Select a load balancer (Create a load balancer).
11. On the Create a load balancer page, specify the load balancer name and take the defaults. Click "Create", when you are done then "Next : Management >".

<p align="center">
<img src="https://github.com/user-attachments/assets/3b0ea859-0477-4867-9d3d-0cb1bd5a24f0">
</p>
 
 12. In the Management tab, set Boot diagnostics to Disable (this saves storage costs and reduces the amount of data stored in your Azure account), then click "Review + Create", ensure validation passes, and click "Create". Note: Wait for the virtual machine scale set deployment to complete. This should take approximately 5 minutes.

<p align="center">
<img src="https://github.com/user-attachments/assets/67c2b9e1-5e0c-4725-bf69-bce39306acae">
</p>

<p align="center">
<img src="https://github.com/user-attachments/assets/28efbc7f-7dfa-47eb-aa05-030e643c509e">
</p>

## Task 4: Adjust the scaling of Azure Virtual Machine Scale Sets.

In this task, you will modify the scale set by applying a custom scaling rule to adjust resource allocation dynamically.

1.	Select "Go to resource" or search for and select the scale set created before.
2.	Choose "Availability + Scale" from the left side menu, then choose "Scaling". In this case a Scale out rule.
3.	Select "Custom autoscale". Then change the Scale mode to "Scale based on metric". And then select "Add a rule".

<p align="center">
<img src="https://github.com/user-attachments/assets/b16fd32e-eba7-4c3a-839a-12b970e240c5">
</p>

4.	Create a rule to automatically scale out VM instances when the average CPU load exceeds 70% over 10 minutes, increasing instances by 20%. Configure: Metric source, Metric namespace (Virtual Machine Host), Metric name (Percentage CPU), Operator (Greater than), Metric threshold (70), Duration (10 minutes), Time grain statistic (Average), Operation (Increase percent by), Cool down (5 minutes), Percentage (50).
5.	Be sure to Add then Save your changes.

<p align="center">
<img src="https://github.com/user-attachments/assets/438d418e-a912-43db-8d01-7430451db1dc">
</p>

6.	Create a scale-in rule to decrease the number of VM instances when the average CPU load drops below 30% over a 10-minute period, reducing instances by 20%. Select "Add a rule", configure: Operator (Less than), Threshold (30), Operation (Decrease percentage by), Percentage (20), then click "Add". Be sure to Save your changes.

<p align="center">
<img src="https://github.com/user-attachments/assets/56b189e3-fe73-4259-94a8-8f0d95a4fc82">
</p>

7.	Set the instance limits to ensure autoscale rules don't exceed the maximum or minimum instances. Configure: Minimum (2), Maximum (10), Default (2). These limits are shown on the Scaling page after the rules. Be sure to Save your changes.

<p align="center">
<img src="https://github.com/user-attachments/assets/c48be039-848a-498c-a061-3c90fd6eaa91">
</p>

8.	To monitor the number of virtual machine instances select "Instances" on the scale set page.
