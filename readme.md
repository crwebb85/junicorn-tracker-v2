# Junicorn Tracker

An instagram tracker that can be deployed to a RaspberryPi. This repository contains the ansible script that can be used to configure a RaspberryPi with the cron jobs needed to backup an Instagram profile's posts and stories. One cron job runs the Instaloader python package to download instagram stories and posts. A second cron job then syncs the files to a Nextcloud file server.

## Setup

1. Setup an Instagram account that you the script will login as to download the files
2. Create a Nextcloud user that the script will login as to backup the files
3. Create an `env.yml` file and place it in the `vars` folder 

The `env.yml` should look like:

```yml
nextcloud_username: yourNextcloudUsername
nextcloud_password: yourNextcloudPassword
nextcloud_url: yourNextcloudUrl
instagram_profile: theInstagramProfileYouWishToBackup
instagram_username: yourInstagramUsername
instagram_password: yourInstagramPassword
```

4. Create an `inventory.ini` file. I place mine in the same directory as the `playbook.yml` file.

The `inventory.ini` file can look like:
```ini
[pie]
<IP Address> ansible_ssh_pass=<SSH Password> ansible_ssh_user=<SSH User> 
```
Replace values denoted with angle brackets with the actual values for your device.

5. Install ansible on your development computer (not the device you want to deploy to)

[Ansible Installation Instructions](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

```bash
python3 -m pip install --user ansible
```

6. Run the ansible script

```bash
ansible-playbook 
-i ./inventory.ini ./playbook.yml --ask-become-pass
```

You will be prompted for the sudo password of the device as the script will need to become sudo to install the needed software.