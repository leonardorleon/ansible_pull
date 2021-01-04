# ansible_pull

Repository for various automated tasks using ansible.

NOTES:

-Git must be installed for ansible to work this way.

-Currently it works on a public repository. Change it such that it works with a private git repository

## Steps on IoT devices

1. Install ansible with

    $sudo apt update && sudo apt install ansible git

2. Run ansible pull for the first time, which will then make it work automatically whenever there is a change on the git repository

    $sudo ansible-pull -U <https://github.com/leo-sigma/ansible_pull.git>

## Ansible repository explained

The ansible repository is there to host the playbooks that are executed by the agents. In this case, the agents will check periodically the repository and if they find changes, they will execute the commands. The following key points are necessary

* local.yml

The structure of the repository contains a local.yml file, which is the first file executed when the devices perform an ansible-pull. This file executes some pre_tasks and then goes on to execute the tasks contained in the tasks directory.

* users.yml

The users.yml creates an user named ansible with the system option set to yes. After this, the second play is to take the sudoers_ansible file from the files directory and locating it into the /etc/sudoers.d/ directory with root as owner and permissions 0440. This file simply adds a sudoer to the machine, to set the ansible user to run commands in the background without personal assistance.

* cron.yml

The cron.yml task is set to execute a cron job for the ansible-pull command every 10 minutes with the user ansible prepared previously. The -o flag makes it such that it will only run when there are changes to the repository. This means it will execute every 10 minutes, but only do something when it sees that there is a change in the git repository.

For now it takes the ouput to /dev/null, maybe it will be changed to a log file but for now it is not needed.

* packages.yml

This will install a few selected packages.

* zabbix.yml

With this play the zabbix agent can get the desired configuration from the files directory.
