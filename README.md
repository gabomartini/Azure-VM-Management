<p align="center">
  <img src="https://github.com/user-attachments/assets/685ab74f-f80c-4e65-a819-2fff4059ae34" width="150" height="auto">
  <h1 align="center">Virtual Machine Management on Azure</h1>
</p>

#### *In this tutorial, we will:*
*•	Set up virtual machines with resilience across availability zones.*

*•	Deploy virtual machines using the Azure portal.*

*•	Configure availability and scaling features for virtual machines.*

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
  <img src="https://github.com/user-attachments/assets/baf4cb5d-fb53-452f-a9c5-c1772028df68">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/941c497c-f999-4552-af0c-bb8f10e441bb">
</p>

4.   Click "Next: Disks >", then configure: OS disk type, Delete with VM (this helps save costs, simplifies resource management, and ensures a clean environment by automatically removing unused disks).

<p align="center">
  <img src="https://github.com/user-attachments/assets/e70ae714-eb0f-4d2e-8693-05911ef0913e">
</p>

5.  Click "Next: Networking >", take the default values except for: Delete public IP and NIC when the VM is deleted (this helps save costs, enhances security, and keeps your Azure environment clean). Click "Review + Create". After validation completes, click "Create". Note: Notice as the virtual machine deploys the NIC, disk, and public IP address (if configured) are independently created and managed resources. Wait for the deployment to complete, then select "Go to resource".

<p align="center">
  <img src="https://github.com/user-attachments/assets/8112c040-3fd3-4d1f-9aa4-2ec7e2ca09e1">
</p>

## Task 2: Manage scaling for compute and storage resources in virtual machines.

In this task, you will resize a virtual machine by selecting a different SKU. Azure allows you to adjust VM specifications as needed, increasing or decreasing compute power and memory. This flexibility also applies to storage, where you can enhance disk performance or expand capacity.

1. On the VM1 virtual machine, in the Availability + scale blade, select "Size".
2. Set the virtual machine size to DS1_v2 and click "Resize". When prompted, confirm the change. Note: Resizing is also known as vertical scaling, up or down.

<p align="center">
 <img src="https://github.com/user-attachments/assets/9d8b1764-ed3a-4d4d-b5e8-42e149cce19c">
</p>

3. In the Settings area, select "Disks".
4.	Under Data disks select "+ Create" and "attach a new disk". Configure the settings and click "Apply" (leave other settings at their default values): Disk name, Storage type (in this case Standard HDD), Size.

<p align="center">
  <img src="https://github.com/user-attachments/assets/84cfe70c-8ad6-4195-8474-951f2f89c7c7">
</p>

5.	After the disk has been created, click "Detach" (if necessary, scroll to the right to view the detach icon), and then click "Apply". Note: Detaching removes the disk from the VM but keeps it in storage for later use.
6.	Search for and select "Disks". From the list of disks, select the VM1 disk object. Note: The Overview blade also provides performance and usage information for the disk.
7.	In the Settings blade, select "Size + performance". Set the storage type to Standard SDD, and then click "Save".

<p align="center">
  <img src="https://github.com/user-attachments/assets/e56b3395-841c-4495-b1cb-4bf5d1fd4dd0">
</p>

8.	Navigate back to the VM1 virtual machine and select "Disks".
9.	In the Data disk section, select "Attach existing disks".
10.	In the Disk name drop-down, select the VM1 disk.

<p align="center">
  <img src="https://github.com/user-attachments/assets/37d03ee3-b730-4acf-9fe3-916bae383f14">
</p>

11.	Verify the disk is now different. Select "Apply" to save your changes. Note: You have now created a virtual machine, scaled the SKU and the data disk size. In the next task we use Virtual Machine Scale Sets to automate the scaling process.

## Task 3: Set up and configure Azure Virtual Machine Scale Sets.

In this task, you will set up an Azure virtual machine scale set spanning multiple availability zones. VM Scale Sets streamline automation by letting you define scaling rules based on metrics, enabling automatic expansion or reduction of resources.

1. In the Azure portal, search for and select "Virtual machine scale sets" and, on the Virtual machine scale sets blade, click "+ Create".
2. On the Basics tab of the Create a virtual machine scale set blade, configure the following settings: Subscription, Resource group, Virtual machine scale set name, Region, Availability zone, Orchestration mode, Security type, Scaling options, Image, Size, Username, Password. Then, go to Networking.
 
<p align="center">
 <img src="https://github.com/user-attachments/assets/e8ec0300-c2b4-410a-a151-030892531d3d">
</p>

 3. On the Networking page, click the "Edit virtual network" link below the Virtual network textbox and create a new virtual network, when finished, select "OK": Network Name, Address range	10.82.0.0/20, Subnetwork name, Subnet range	10.82.0.0/24.

<p align="center">
<img src="https://github.com/user-attachments/assets/d30e10e3-41db-4983-87de-9c42417a2e22">
</p>

<p align="center">
 <img src="https://github.com/user-attachments/assets/4be42df2-082d-4d96-ae20-70d79f5e46e2">
</p>

4. In the Networking tab, click the "Edit network interface" icon to the right of the network interface entry.
5. For NIC network security group section, select "Advanced" and then click "Create new" under the Configure network security group drop-down list.

 <p align="center">
 <img src="https://github.com/user-attachments/assets/1c450a62-5aa9-4a82-ac0c-cac37b17dbf8">
</p>

6. On the Create network security group blade, specify the name (leave others with their default values).
7. Click "Add an inbound rule" and configure: Source (Any), Source port ranges (*), Destination (Any), Service (HTTP), Action (Allow), Priority (1010), Name (allow-http), then save the rule.

<p align="center">
<img src="https://github.com/user-attachments/assets/430f0720-29f4-45c6-b052-df9e90028145">
</p>
 
8. Click "Add" and, back on the Create network security group blade, click "OK".
9. In the Edit network interface blade, in the Public IP address section, click "Enabled" and click "OK".
10. In the Networking tab, under the Load balancing section, configure: Load balancing options (Azure load balancer), Select a load balancer (Create a load balancer).
11. On the Create a load balancer page, specify the load balancer name and take the defaults. Click "Create", when you are done then "Next : Management >".

<p align="center">
<img src="https://github.com/user-attachments/assets/8a31b3b9-be4b-4a6f-a697-a6903dd9d50d">
</p>
 
 12. In the Management tab, set Boot diagnostics to Disable (this saves storage costs and reduces the amount of data stored in your Azure account), then click "Review + Create", ensure validation passes, and click "Create". Note: Wait for the virtual machine scale set deployment to complete. This should take approximately 5 minutes.

<p align="center">
<img src="https://github.com/user-attachments/assets/35bba938-933b-45a1-9783-c460f0d0c892">
</p>

<p align="center">
<img src="https://github.com/user-attachments/assets/a4c747a9-9270-4150-8c06-1bbe892c436a">
</p>

## Task 4: Adjust the scaling of Azure Virtual Machine Scale Sets.

In this task, you will modify the scale set by applying a custom scaling rule to adjust resource allocation dynamically.

1.	Select "Go to resource" or search for and select the scale set created before.
2.	Choose "Availability + Scale" from the left side menu, then choose "Scaling". In this case a Scale out rule.
3.	Select "Custom autoscale". Then change the Scale mode to "Scale based on metric". And then select "Add a rule".

<p align="center">
<img src="https://github.com/user-attachments/assets/40331034-35f8-48ee-ab1a-1c1f22fc64a7">
</p>

4.	Create a rule to automatically scale out VM instances when the average CPU load exceeds 70% over 10 minutes, increasing instances by 20%. Configure: Metric source, Metric namespace (Virtual Machine Host), Metric name (Percentage CPU), Operator (Greater than), Metric threshold (70), Duration (10 minutes), Time grain statistic (Average), Operation (Increase percent by), Cool down (5 minutes), Percentage (50).
5.	Be sure to Add then Save your changes.

<p align="center">
<img src="https://github.com/user-attachments/assets/fb366df5-451c-4437-9455-1d983c001404">
</p>

6.	Create a scale-in rule to decrease the number of VM instances when the average CPU load drops below 30% over a 10-minute period, reducing instances by 20%. Select "Add a rule", configure: Operator (Less than), Threshold (30), Operation (Decrease percentage by), Percentage (20), then click "Add". Be sure to Save your changes.

<p align="center">
<img src="https://github.com/user-attachments/assets/05646fd7-c109-46c2-8c7f-2532efd324dd">
</p>

7.	Set the instance limits to ensure autoscale rules don't exceed the maximum or minimum instances. Configure: Minimum (2), Maximum (10), Default (2). These limits are shown on the Scaling page after the rules. Be sure to Save your changes.

<p align="center">
<img src="https://github.com/user-attachments/assets/0d286a67-b948-4d70-9cb7-66eabd21ed3e">
</p>

8.	To monitor the number of virtual machine instances select "Instances" on the scale set page.
