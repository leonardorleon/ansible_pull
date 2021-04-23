# iot_ansible_pull

Repository for various automated tasks on multiple raspberry pis using ansible. This is a test or demo on deployment of IoT devices using ansible pull. Given that in many locations it is not often possible to simply "push" instructions or updates to the devices, for example due to limitations on how a client's network might be set up. In other cases the device might not be online all the time and it is convenient for it to serve itself whenever it is able to. I found ansible pull to be a decent way of setting up devices with minimal effort and maintaining configurations in a central location.

I explored using zabbix for monitoring purposes on raspberry pis, so I played around with installing and configuring it with just one command on ansible. Naturally, it is also meant to work in "pull mode", so the device would give the zabbix server when it can instead of the zabbix server requesting it. 

NOTES:

-Git must be installed for ansible to work this way.

-Currently it is working on a public repository. Depending on the use case it should be changed to work on a private git repo.

## Steps on IoT devices

1. Install ansible with

    $sudo apt update && sudo apt install ansible git

2. Run ansible pull for the first time, which will then make it work automatically whenever there is a change on the git repository

    $sudo ansible-pull -U <GIT_REPOSITORY>

## Ansible repository explained

The ansible repository is there to host the playbooks that are executed by the agents. In this case, the agents will check periodically the repository and if they find changes, they will execute the commands. The following key points are necessary

* local.yml

The structure of the repository contains a local.yml file, which is the first file executed when the devices perform an ansible-pull. This file executes some pre_tasks and then goes on to execute the tasks contained in the tasks directory.

* inventory.yml

Here is a simple list of the host groups and devices, as well as variables tied to them wherever needed.

* ansible.cfg

For now it simply directs ansible to the inventory.yml file to correclty load the information and structure of the devices.

# roles

This directory holds a couple of test roles used for some basic tasks which can be added to all raspberry pi and some extra tasks which might come in handy on certain devices. This being a demo, they simply print some values or variables and install some packages.

# Tasks

* users.yml

The users.yml creates an user named ansible with the system option set to yes. After this, the second play is to take the sudoers_ansible file from the files directory and locating it into the /etc/sudoers.d/ directory with root as owner and permissions 0440. This file simply adds a sudoer to the machine, to set the ansible user to run commands in the background without personal assistance.

* cron.yml

The cron.yml task is set to execute a cron job for the ansible-pull command every 10 minutes with the user ansible prepared previously. The -o flag makes it such that it will only run when there are changes to the repository. This means it will execute every 10 minutes, but only do something when it sees that there is a change in the git repository.

For now it takes the ouput to /dev/null, maybe it will be changed to a log file but for now it is not needed.

* packages.yml

This will install a few selected packages.

* zabbix.yml

With this play the zabbix agent can get the desired configuration from the files directory.
