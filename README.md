NetAutHardWay

Network Automation Examples Using Ansible "command" and "raw" modules

# Network Automation - Hard Way
This repository includes files to demonsrate how Ansible "raw" and "command" modules can be used to start with network automation.

Lab topology is based on Ivan\`s vEOS Leaf/Spine architecture. 
I just added two Tiny Core VMs to simulate customer end points connected to by VxLAN Fabric.
Tiny Cores helps when we want to create MAC/IP reports of our fabric.

# Why the Hard Way
I tried to watch each video of Ivan\`s course and take notes. At the same time I also started a new job and did not have enough time
to finish course in a few months. It took more than 10 months for me to finish the course. Eventually I forgot what I learnt and had to 
go back/forward within my notes when I was trying to remember and connect the pieces.
At the end of the day I decided to progress slowly and see the different ways of automationg a network.
I will start with primitive automation and use Ansible "raw" and "command" modules first.
Then I will move to Ansible "network modules".
Finally I will try "NAPALM".
Under each part, there will be some parts like reports, configuration, CI/CD extension etc.
At least this is the plan. Let\`s see how much I can proceed.

# How to Use
1. Lab: 
includes files requited for lab setup. More details available on Ivan\`s git repository.

2. Step1-report:
find_ip.yml: collects and saves arp tables of a customer in a file and shows where the specified IP address is connected   
find_mac.yml: collects and saves mac tables of a customer in a file and shows where the specified MAC address is connected    
get_cust-ports.yml: collects and saves interfaces descriptions of the leaf switches and gives the list of service access points for the specified customer    

3. Step2-config:
This section creates underlay, overlay and services configurations using roles.
create_configs.yml merges these configuration and creates the device configuration files.
config_delivery.yml uses multiple playbooks including create_configs to create partial/full configs, to push them to the devices, to show diff between running config and pushed configs and finally commit the pushed configurations to the devices and return the commit result.


