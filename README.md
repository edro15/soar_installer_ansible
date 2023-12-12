# SOAR Installer Ansible
This ansible playbook installs Splunk SOAR on remote machines running Centos7.

Tested with SOAR 5.3.1, 6.1.0 and 6.2.0

## Requirements
On the remote machine:
- CentOS 7
- Internet access (to download the SOAR image)

On your local machine:
- Ansible installed

	`brew install ansible` for OSX

## Usage
- Clone repository to your directory

- Create a new instance with i.e Nova EC2 with such parameters
	- Instance Type	`m5.large` for faster installing
	- Storage 100 GiB
	- OS CentOS 7

- Copy your local ssh public key to the destination host(s)

	```ssh-copy-id -i ~/.ssh/id_rsa.pub $USERNAME@$HOSTADDR``` 

- Configure your inventory `inventory.yaml`
	- Add the address of the host(s) 
    - Go to [phantom-downloader](https://my.phantom.us/downloads/), authenticate, and copy the link to SOAR version which you need to install
        > Remember that token for link expires after ~10min
	- Paste this link to `soar_download_url`
    
    If your version is currently not supported, add a new group similarly to the others

- Verify `roles/soar/vars/main.yaml`
	- Add the ansible user (i.e splunker for NOVA)
	- Enter desired password for the user created by SOAR (`phantom`) to `phantom_user_password`
    
    These values take precedence over equivalent variables added in `inventory.yaml` under `vars`. 

- Run the playbook
    - To install **multiple versions of SOAR** in configured hosts at once
	```ansible-playbook -i inventory.yaml launcher.yaml```

    - To install **one version of SOAR** in configured hosts. Example for SOAR 6.1.0
	```ansible-playbook -i inventory.yaml -l group_610 launcher.yaml```

- Wait for it :smile:

>   - The installation will take ~4-5 minutes. You can step away to get your coffee.
>	- By default, SOAR will run on port 8443 over HTTPS.
>	- The *default login credentials* are `admin/password`. For version 6.0.0 and higher: `soar_local_admin/password`

## Troubleshooting
- Execute commands manually in the machine
- Run the playbook in verbose mode
	```ansible-playbook -i inventory.yaml launcher.yaml -vvv```
