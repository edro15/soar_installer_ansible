# soar_installer_ansible
Ansible installation script for SOAR instances on CentOS7 hosts

## Introduction
This ansible playbook installs Splunk SOAR on remote machines running Centos7.

## Requirements:
- OS: CentOS 7
- Internet access on the remote machine (to download the SOAR image)
- Set the required parameters/variables in:

	hosts.yaml:

		remote host username (can also be passed via the CLI)


	vars.yaml:

		- SOAR image URL (get the "authenticated" download link from: https://my.phantom.us/downloads/)
		- desired password for `soaruser`: default is `soaruser`
	


## Instructions:
- Clone repository to your directory
	```
	cd projects_directory
	git clone link name_of_folder
	cd name_of_folder
	```

- Create a new instance with i.e Nova EC2 with such parameters:
	- Instance Type	m5.large for faster installing
	- Storage 100 GiB
	- OS centos 7

- Copy your local ssh public key to the destination host(s):
  
	```ssh-copy-id -i ~/.ssh/id_rsa.pub $USERNAME@$HOSTADDR``` 

- Set hosts file
	Add the address of the host(s) and ansible user (i.e soaruser) to the included `hosts.yaml` file.

- Set vars file
	- Go to [phantom-downloader](https://my.phantom.us/downloads/) and copy link to SOAR version which you need
		- Hint! - remember that token for link expired after ~10min
	- Paste this link to soar_download_url
	- Enter password

- Run the playbook

	 - Hint! - be aware what version of SOAR are you installing, if this is under 5.3v `Run installer` task should look like that:

	```bash
	 - name: Run installer
		become: yes
		become_user: soaruser
		shell: "/tmp/phantom_tar_install.sh install"
	```
	
	```ansible-playbook -i hosts.yaml fdse_soar_installer.yaml```

- Wait for it :)

	By default, SOAR will run on port 8443 over HTTPS.
	The *default login credentials* are `admin/password`, for version 6.0.0 and higher `soar_local_admin/password`
