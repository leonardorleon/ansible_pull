# ansible_pull

Repository for various automated tasks using ansible.

NOTES:
-change it such that it works with a private git repository

## Steps on IoT devices

1. Install ansible with

    $sudo apt update && sudo apt install ansible

2. Run ansible pull for the first time, which will then make it work automatically whenever there is a change on the git repository

    $sudo ansible-pull -U <https://github.com/leo-sigma/ansible_pull.git>
