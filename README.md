*NetAutHardWay*

*Network Automation Examples Using Ansible "command", "raw" and "shell" modules*

# Network Automation - Hard Way
This repository includes files to demonstrate how Ansible "raw" and "command" modules can be used to start with network automation.

Lab topology is based on Ivan\`s vEOS Leaf/Spine architecture. 
I just added two Tiny Core VMs to simulate customer end points connected to by VxLAN Fabric.
Tiny Cores helps when we want to create MAC/IP reports of our fabric.

# Why the Hard Way
I tried to watch each video of Ivan\`s course (100+ hours in total) and take notes. At the same time I also started a new job and did not have enough time
to finish course in a few months. It took more than 10 months for me to finish the course. Eventually I forgot what I learnt and had to 
go back/forward within my notes when I was trying to remember and connect the pieces.

At the end of the day I decided to progress slowly and see the different ways of automating the network mentioned in the course.
I will start with primitive automation and use Ansible "raw" and "command" modules first.
Then I will move to Ansible "network modules".
Finally I will try "NAPALM".

Under each part, there will be some parts like reports, configuration, CI/CD extension etc.
At least this is the plan. Let\`s see how much I can advance.

# How to Use
1. Lab: 

includes files requited for lab setup. More details available on Ivan\`s git repository: https://github.com/ipspace/NetOpsWorkshop/tree/master/topologies/EOS-Leaf-and-Spine
As mentioned before, my lab is based on above lab and includes additional Tiny Core VMs, which consume less than 64MB RAM each and very useful to generate MAC/IP addresses connected to the lab. They also support trunking with iproute2 utilities.

```
spine-1      spine-2
  |    \    /   |
  |     \  /    |
  |      \/     |
  |      /\     |
  |     /  \    |
  |    /    \   |  
leaf-1        leaf-2
  |             |
tiny-1        tiny-2
```

2. Step1-report:

- find_ip.yml: collects and saves arp tables of a customer in a file and shows where the specified IP address is connected (switch name + port)
- find_mac.yml: collects and saves mac tables of a customer in a file and shows where the specified MAC address is connected switch name + port)   
- get_cust-ports.yml: collects and saves interfaces descriptions of the leaf switches and gives the list of service access points for the specified customer (switch name + port)      

3. Step2-config:

This section creates underlay, overlay and services configurations using roles, scp them to the devices, shows the diff and finally commits the changes:
- config_delivery.yml uses multiple playbooks including:
  - create_configs to create partial/full configs and merge them
  - push_configs to push(scp) them to the devices
  - diff_configs to see the differences between running config and pushed config
  - commit_configs to commit the configuration changes to the devices and to return the commit result.

3. Step3-git:

This section creates a new branch using the date as the branch name and reports saves configuration discrepancies between sto-be-config and actual-config by repeating the Step2 and making a diff using the Arista config session feature:
- git_discrepancy_report uses multiple playbooks including:
  - git_create_diff_branch.yml to pull the latest repo from GitHub and create a new branch
  - create_configs and push_configs from Step2
  - save_config_diffs.yml to report the diff for each device
  - git_push_config_changes.yml to report the discrepancies by pushing the new branch to the GitHub

4. Step4-CICD:

This section tries to implement basic SW Development Practices in Network Change Management. I tried to use Jenkins pipeline approach to run some tests automatically on the network configuration described as YAML/Jinja files. I am also using the same virtual lab for integration and delivery tests and I do not fire up a test lab on the fly for these tests because unlike many others\` lab, this lab takes 10+ minutes to come up on my laptop.
- Integration: Tests on the feature branch (in my case the branch called "jenkinsbranch")
  - basic bgp and ping connectivity tests to make sure that fabric is configured correctly. Another VLAN test to make sure services      configuration for customer are deployed correctly (Here I check if each customer VLAN/Port is implemented as described in the services YAML file)
- Delivery: Tests after I pull the master at GitHub to my feature branch. Basically I repeat the above tests after I merge my feature branch with the latest master branch.

There will be an approval required at this stage to proceed further. Then the local branch will be pushed to the GitHub master branch.
Note: Next steps are not working as expected. Pipiline is broken at the moment.

- Deployment: In real life this would go and deploy the configuration on production and perform the same tests in the above steps.
- Post-Deployment: In real life this would go  and check the status of customer ports and whether we see any MAC addresses on those ports.
