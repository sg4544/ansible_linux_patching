# ansible_linux_patching
Ansible roles to apply security/patch updates to Linux servers on Vmware

Prerequisites:
01 - Create patch_variables.yml vault file:
     ansible-vault create patch_variables.yml
     Add below variables to the file:
         VCenter01: <vcenter fqdn>
         VMware_username: <vcenter username>
         VMware_password: <vcenter user password>
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
