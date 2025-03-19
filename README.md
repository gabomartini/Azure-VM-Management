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

Deploy two Azure virtual machines (VMs) across different availability zones using the Azure portal. Availability zones (isolated locations within a region) ensure maximum uptime, offering a 99.99% SLA when at least two VMs are deployed in separate zones.

1.	In the Azure portal, search for and select "Virtual machines".
2.	On the "Virtual machines" blade, click "+ Create", then select "Azure virtual machine" from the drop-down menu.
3.	On the "Basics" tab, configure the following settings:
    -  Subscription: Select your subscription.
    -  Resource group: Choose or create a resource group.
    -  Virtual machine names: Enter names for two VMs (e.g., "VM1" and "VM2") after selecting availability zones.
    -  Region: Select your preferred region (e.g., "(US) East US").
    -  Availability options: Choose "Availability zone".
    -  Availability zone: Check "Zone 1" and "Zone 2" to distribute the VMs.
    -  Security type: Select the default or your preferred option.
    -  Image: Choose an OS image (e.g., "Windows Server 2022").
    -  Azure Spot instance: Set to "No" (unless using Spot VMs).
    -  Size: Select "Standard_DS2_v3" (or another size as needed).
    -  Username: Enter an admin username.
    -  Password: Enter a strong password.
    -  Public inbound ports: Select "Allow selected ports" (e.g., RDP - 3389).

<p align="center">
  <img src="https://github.com/user-attachments/assets/baf4cb5d-fb53-452f-a9c5-c1772028df68">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/941c497c-f999-4552-af0c-bb8f10e441bb">
</p>

4.	Click "Next: Disks >".
5.	On the "Disks" tab, configure:
    -  OS disk type: Select "Standard SSD" (or another type as needed).
    -  Delete with VM: Check this box to automatically remove disks when the VM is deleted (saves costs and simplifies cleanup).

<p align="center">
  <img src="https://github.com/user-attachments/assets/e70ae714-eb0f-4d2e-8693-05911ef0913e">
</p>

6.	Click "Next: Networking >".
7.	On the "Networking" tab, accept the default values except:
    -  Delete public IP and NIC when VM is deleted: Check this box to remove associated resources when the VM is deleted (enhances security and reduces costs).
8.	Click "Review + Create". After validation passes, click "Create".
9.	Wait for the deployment to complete. (Note: The NIC, disk, and public IP address, if configured, are created as independent resources.)
10.	Once deployment finishes, click "Go to resource" to view the VM.

<p align="center">
  <img src="https://github.com/user-attachments/assets/8112c040-3fd3-4d1f-9aa4-2ec7e2ca09e1">
</p>

## Task 2: Manage scaling for compute and storage resources in virtual machines.

Resize a virtual machine by changing its SKU (Stock Keeping Unit, defining compute power and memory) and adjust storage by adding and modifying a data disk. Azure allows flexible scaling of compute and storage resources as needed.

1.	Navigate to the "VM1" virtual machine created in Task 1.
2.	In the "Availability + scale" blade, select "Size".
3.	Set the virtual machine size to "Standard_DS1_v2", then click "Resize". Confirm the change when prompted. (Note: Resizing, or vertical scaling, adjusts compute power up or down.)

<p align="center">
 <img src="https://github.com/user-attachments/assets/9d8b1764-ed3a-4d4d-b5e8-42e149cce19c">
</p>

4.	In the "Settings" area, select "Disks".
5.	Under "Data disks", click "+ Create and attach a new disk".
6.	Configure the new disk settings, then click "Apply":
    -  Disk name: Enter a name (e.g., "VM1-DataDisk").
    -  Storage type: Select "Standard HDD".
    -  Size: Choose a size (e.g., 32 GiB). Leave other settings at default values.

<p align="center">
  <img src="https://github.com/user-attachments/assets/84cfe70c-8ad6-4195-8474-951f2f89c7c7">
</p>

7.	After the disk is created, click "Detach" (scroll right to find the detach icon if needed), then click "Apply". (Note: Detaching removes the disk from the VM but retains it in storage.)
8.	Search for and select "Disks" in the Azure portal.
9.	From the list of disks, select the VM1 disk object. (Note: The Overview blade shows performance and usage details.)
10.	In the "Settings" blade, select "Size + performance".
11.	Change the storage type to "Standard SSD", then click "Save".

<p align="center">
  <img src="https://github.com/user-attachments/assets/e56b3395-841c-4495-b1cb-4bf5d1fd4dd0">
</p>

12.	Navigate back to the "VM1" virtual machine and select "Disks".
13.	In the "Data disks" section, click "Attach existing disks".
14.	In the "Disk name" drop-down, select the VM1 disk, then click "Apply".

<p align="center">
  <img src="https://github.com/user-attachments/assets/37d03ee3-b730-4acf-9fe3-916bae383f14">
</p>

15.	Verify the disk is attached with the updated "Standard SSD" type. (Note: You’ve now scaled the VM’s SKU and data disk.)

## Task 3: Set up and configure Azure Virtual Machine Scale Sets.

Set up an Azure Virtual Machine Scale Set (VMSS), which automates scaling across multiple VMs in availability zones based on defined rules.

1.	In the Azure portal, search for and select "Virtual machine scale sets".
2.	On the "Virtual machine scale sets" blade, click "+ Create".
3.	On the "Basics" tab of the "Create a virtual machine scale set" blade, configure:
    -  Subscription: Select your subscription.
    -  Resource group: Choose or create a resource group.
    -  Virtual machine scale set name: Enter a name (e.g., "VMSS1").
    -  Region: Select your preferred region (e.g., "(US) East US").
    -  Availability zone: Check "Zone 1" and "Zone 2".
    -  Orchestration mode: Select "Uniform".
    -  Security type: Use the default or your preferred option.
    -  Scaling options: Select "Manual" (adjusted in Task 4).
    -  Image: Choose an OS image (e.g., "Windows Server 2022").
    -  Size: Select a size (e.g., "Standard_DS2_v3").
    -  Username: Enter an admin username.
    -  Password: Enter a strong password.
 
<p align="center">
 <img src="https://github.com/user-attachments/assets/e8ec0300-c2b4-410a-a151-030892531d3d">
</p>

4.	Click "Next: Networking >".
5.	On the "Networking" tab, click "Edit virtual network" below the "Virtual network" textbox.
6.	Create a new virtual network, then click "OK":
    -  Network Name: Enter a name (e.g., "VMSS-VNet").
    -  Address range: Set to "10.82.0.0/20".
    -  Subnet name: Enter "default".
    -  Subnet range: Set to "10.82.0.0/24".

<p align="center">
<img src="https://github.com/user-attachments/assets/d30e10e3-41db-4983-87de-9c42417a2e22">
</p>

<p align="center">
 <img src="https://github.com/user-attachments/assets/4be42df2-082d-4d96-ae20-70d79f5e46e2">
</p>

7.	Click the "Edit network interface" icon next to the network interface entry.
8.	In the "NIC network security group" section, select "Advanced", then click "Create new" under the "Configure network security group" drop-down.

 <p align="center">
 <img src="https://github.com/user-attachments/assets/1c450a62-5aa9-4a82-ac0c-cac37b17dbf8">
</p>

9.	On the "Create network security group" blade, enter a name (e.g., "VMSS-NSG"), then click "Add an inbound rule", configure:
    -  Source: "Any".
    -  Source port ranges: "*".
    -  Destination: "Any".
    -  Service: "HTTP".
    -  Action: "Allow".
    -  Priority: "1010".
    -  Name: "allow-http".
    -  Click "Add"

<p align="center">
<img src="https://github.com/user-attachments/assets/430f0720-29f4-45c6-b052-df9e90028145">
</p>
 
10.	On the "Create network security group" blade, click "OK".
11.	In the "Edit network interface" blade, under "Public IP address", click "Enabled", then click "OK".
12.	On the "Networking" tab, under "Load balancing", configure:
    -  Load balancing options: Select "Azure load balancer".
    -  Select a load balancer: Click "Create a load balancer".
13.	On the "Create a load balancer" page, enter a name (e.g., "VMSS-LB"), accept defaults, and click "Create".
14.	Click "Next: Management >".

<p align="center">
<img src="https://github.com/user-attachments/assets/8a31b3b9-be4b-4a6f-a697-a6903dd9d50d">
</p>
 
15.	On the "Management" tab, set "Boot diagnostics" to "Disable" (saves storage costs), then click "Review + Create".
16.	After validation passes, click "Create". Wait for the deployment to complete (approximately 5 minutes).

<p align="center">
<img src="https://github.com/user-attachments/assets/35bba938-933b-45a1-9783-c460f0d0c892">
</p>

<p align="center">
<img src="https://github.com/user-attachments/assets/a4c747a9-9270-4150-8c06-1bbe892c436a">
</p>

## Task 4: Adjust the scaling of Azure Virtual Machine Scale Sets.

Modify the VM Scale Set by adding custom scaling rules to dynamically adjust resource allocation based on CPU usage.

1.	Navigate to the VM Scale Set created in Task 3 (e.g., "VMSS1") by clicking "Go to resource" or searching for it.
2.	In the left menu, select "Availability + Scale", then click "Scaling".
3.	Choose "Custom autoscale", then set "Scale mode" to "Scale based on a metric".

<p align="center">
<img src="https://github.com/user-attachments/assets/40331034-35f8-48ee-ab1a-1c1f22fc64a7">
</p>

4.	Click "Add a rule" to create a scale-out rule. Configure the rule to increase VM instances by 20% when the average CPU load exceeds 70% over 10 minutes:
    -  Metric source: Current resource.
    -  Metric namespace: "Virtual Machine Host".
    -  Metric name: "Percentage CPU".
    -  Operator: "Greater than".
    -  Metric threshold: "70".
    -  Duration: "10 minutes".
    -  Time grain statistic: "Average".
    -  Operation: "Increase percent by".
    -  Percentage: "20".
    -  Cool down: "5 minutes".
    -  Click "Add".

<p align="center">
<img src="https://github.com/user-attachments/assets/fb366df5-451c-4437-9455-1d983c001404">
</p>

5.	Click "Add a rule" to create a scale-in rule. Configure the rule to decrease VM instances by 20% when the average CPU load drops below 30% over 10 minutes:
    -  Operator: "Less than".
    -  Threshold: "30".
    -  Operation: "Decrease percent by".
    -  Percentage: "20".
    -  Click "Add".

<p align="center">
<img src="https://github.com/user-attachments/assets/05646fd7-c109-46c2-8c7f-2532efd324dd">
</p>

6.	Set instance limits on the "Scaling" page:
    -  Minimum: "2".
    -  Maximum: "10".
    -  Default: "2".

<p align="center">
<img src="https://github.com/user-attachments/assets/0d286a67-b948-4d70-9cb7-66eabd21ed3e">
</p>

7.	Click "Save" to apply the scaling rules and limits.
8.	To monitor the number of VM instances, select "Instances" on the scale set page.
