# Ansible

## Setup the environment
Download and install latest version of VirtualBox.
  - https://www.virtualbox.org/wiki/Downloads

Download CentOS 7 (latest the better) from OS Boxes website.
  - https://www.osboxes.org/centos

Create the "template" VM with following settings.
  - Name: centos-template.
  - Type: Linux.
  - Version: Other Linux (64-bit).
  - Memory: 2048.
  - Use an existing VHD option and select the image downloaded.
  - Create the VM.

Change the following settings before starting the VM.
  - System > Processors: 2.
  - Network > Adapter 1 > Attached to: Bridge Adapter.

Power on the VM. Use the username password details specified in the OS Boxes website for the image downloaded (under info tab).
  - Username: osboxes
  - Password: osboxes.org

Once, logged into the VM open a terminal and issue an "ifconfig" command to reveal its IP Address. Then, use the Mac terminal or any other SSH connection tool to make sure we can SSH into it. For Macs: Use command "ssh osboxes@IP_ADDRESS" and enter the password "osboxes.org" when prompted.

Right-click on the "centos-template" VM to create the Ansible controller node with the following settings.
  - Name: ansiblecontroller
  - MAC Address Policy: Generate new MAC address for all network adaptors.
  - Clone type: Linked clone.
  - Click on Clone.

Create 2 more VMs to be used as Ansible target nodes (2). Just follow the same steps used above to create the controller node.
  - ansible-target1.
  - ansible-target2.

Follow these steps in the controller node and 2 target nodes to change their hostnames.
  - Start the VM.
  - Run "ifconfig" command in a terminal and note the IP Address of it.
  - Start a SSH session using Mac terminal or any other SSH tool.
    - Username: osboxes
    - Password: osboxes.org
  - Change the hostnames file.
    - sudo vi /etc/hostnames
    - Press the key "I" to enter the edit mode.
    - Change value to ansiblecontroller / target1 / target2 (depending on the VM).
    - Press ESC, and type ":wq!" to save the file.
  - Change the hosts file.
    - sudo vi /etc/host
    - Press the key "I" to enter the edit mode.
    - Replace the current values with ansiblecontroller / target1 / target2 (depending on the VM).
    - Press ESC, and type ":wq!" to save the file.
  - Restert the VM.
    - sudo shutdown now -r

After restarting the VMs we should see the hostnames have been updated from "osboxes" to the values we specified.

Install Ansible latest package on the controller node ONLY.
  - sudo yum install ansible
  - If the installation gives the "No package ansible available" error then, install the "epel-release" package before trying to install Ansible again.
    - sudo yum install epel-release
  - Make sure Ansible is correctly installed and check the version.
    - ansible --version

Remote node computers must be accessible from the control node through SSH otherwise, Ansible can't operate! 

## Ansible Inventory
Ansble inventory file lists the set of named remote target nodes that needs to be managed from this Ansible controller. We can group nodes by naming them - [db]. Groups can be grouped again to create bigger groups.

Ansible command could then be used to control and test nodes by using it's name in the inventory file.
 - ansible node_name -m ping -i inventory.txt

## Ansible Playbooks
Download and install VSCode IDE for composing Ansible playbook YAML files. The YAML extension by RedHat is very useful to parse YAML files and catch errors in them!

The yamllint.com website is an external resource to parse YAML files.

The ftp-sync VSCode extension is good to sync local YAML files to the remote Ansible controller computers.

Once, the YAML playbook has been composed you can run it by specifying the inventory file.
- ansible-playbook playbook-pingtest.yaml -i inventory.txt

