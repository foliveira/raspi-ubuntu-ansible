# ansible-test
Raspberry Pi Ubuntu lab provisioning with Ansible

# Overview: 
Repo contains necessary files to provision multiple Ubuntu Raspberry Pi in home lab. Each Pi SD card is flashed with the latest Ubuntu Server image for Ras Pi, and a user-data file with customized hostname is copied to boot partition. Once booted, cloud-init with the custom user-data file will set the hostname, install Avahi & Ansible, create an 'ansible' user with associated ssh key, and use ansible-pull to run the playbooks from this repo.

Ansible will further configure host networking (based on assigned hostname and assigned role & variables), setting IP, netmask, gateway, DNS server(s), and DNS search suffix(es) by creating a host-specific Netplan file from the included template.  Ansible will set the timezone (from variable) and, based on assigned role (here 'standalone' or 'kubernetes'), install Docker & CIFS-Utils (custom need here), and make the Raspberry Pi devices ready for further configuration or package installations as appropriate.

Cloud-init portion based somewhat on https://github.com/StefanScherer/rpi-ubuntu, I like Raspberry Pi Imager (https://www.raspberrypi.org/documentation/installation/installing-images/) instead of flash for ability to run from Windows, but at the cost of not being able to flash user-data file in same operation.

This replaces my Terraform bootstrap (https://github.com/clayshek/terraform-raspberrypi-bootstrap) for ease of use and combining provisionsing with some initial configuration.

# Usage:
- Contents here reflect my lab infrastructure, but with a few modifications should be portable elsewhere.
- Simply flash Raspberry Pi SD card with desired Ubuntu Server version: https://ubuntu.com/download/raspberry-pi
- Copy user-data to boot partition of SD card (first customize hostname value, which should be reflected in Ansible inventory.yml file and have a .yml variables file in host_vars, and do any user/ssh key modifications as necessary)
- Boot Raspberry Pi, after a few minutes, Pi should be provisioned 
- Troubleshoot logs at /var/log/cloud-init-output.log & /var/log/ansible-provision.run
