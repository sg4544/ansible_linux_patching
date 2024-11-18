# ansible_linux_patching

Ansible roles to apply security/patch updates to Linux servers running on Vmware.

Features
--------


01 - Shutdown running applications/DB on the VM

02 - Perform Vmware Snapshot of the VM

03 - Option to apply only security updates, or only bugfixes or all available updates

04 - Reboot the system (optional, only if needed)

05 - Verify upgraded patches/kernel

06 - Start applications/DB on the VM


Prerequisites:
--------------

01 - Create patch_variables.yml vault file:
     ansible-vault create patch_variables.yml
     
     Add below variables to the file:
         VCenter01: <vcenter fqdn>
         VMware_username: <vcenter username>
         VMware_password: <vcenter user password>
         ansible_user: <ssh_username>
         ansible_password: <ssh_password>
         ansible_become_user: root
         patch_reboot_timeout: 30                    # in seconds
         patch_scope: all|security|bugfix            # choose one
         patch_auto_reboot: 1|0                      # choose one 
         excluded_pkgs:                              # specify list
         - postgres-*
         - mysql-*
         - xyz-*

02 - Create below files on the Linux VM. This appraoch ensures that the Ansible routines is abstracted from the type of application running on the VM and the stop/start for the local application/DB is managed locally on the VM

     sudo mkdir /etc/startstop
     sudo touch /etc/startstop/stopapps.sh
     sudo touch /etc/startstop/startapps.sh

     Sample startapps.sh
     #!/bin/bash
     sudo systemctl apache2 start

     Sample stopapps.sh
     #!/bin/bash
     sudo systemctl apache2 stop

03 - Create a local inventory file with list of Linux servers to update

04 - Run the playbook as below:

     ansible-playbook -i inventory_file -k -K --check apply_linux_patches.yml    # Test Mode
     ansible-playbook -i inventory_file -k -K apply_linux_patches.yml            # Apply Mode
     
