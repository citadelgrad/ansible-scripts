# Ansible Setup

## Client & Manager Setup Script

To run ansible scripts you must have Ansible installed on the manager and the client. The manager is the computer you run script from and can be your workstation or a dedicated server. Clients are any system you want to run ansible scripts against.

We are installing ansible, configuring local user accounts with permissions, and copy the managers ssh key to the client. Both of the scripts run locally only.

### Step 1 - Install Ansible on your OS

These instructions show you how to setup Ansible on linux. Configuring Windows authentication involves a different process.

#### Ubuntu setup

```
sudo apt -y update
sudo apt -y install ansible
```

#### CentOS setup

``` 
sudo yum update
sudo yum install ansible
```

#### Alpine setup

```
apk update
sudo apk add ansible
```

### Step 2

We want to modify the Ansible hosts file to specify the local connection. Edit the hosts file and include local connection information.

```
sudo nano /etc/ansible/hosts

# /etc/ansible/hosts
[local]
127.0.0.1 ansible_connection=local
```

### Step 3

The setup process involves creating a local ansible user account on both the manager and the client. This allows the manager to connect to each client using it’s ssh key.

#### Manager (Server) Setup

When we setup the manager account with this script, no password is set because you won’t be logging in to the manager account with Ansible.

`ansible-playbook manager_ansible_setup.yml`

#### Client Setup

> For each client, you will want to repeat steps 3 and 4 for systems you will manage.

Run this command to setup the client and use your own password.

`ansible-playbook client_ansible_setup.yml --extra-vars "ansible_password=<your password>"`

### Step 4

In this step, we want to copy the managers public ssh key to each client under the ansible account. Every time our manager will connect to our host, it will do so using the ansible user account on

Switch to the Server’s Ansible user

`sudo su - ansible`

On server, run command to add SSH Key to client 

`ssh-copy-id <hostname/IP>`

You will be asked to provide the client machines ansible username you set in the previous step.
