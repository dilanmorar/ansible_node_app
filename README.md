# ansible nodejs app

This project uses ansible to provision a nodejs app and a mongo database onto virtual machines using playbooks. It will link the app and database together. This project already contains the code for the app so the aim of this is to use ansible.

## Ansible

 Ansible is an open-source software provisioning, configuration management and application deployment tool. It is designed for multi-tier deployments. It uses a simple language YAML which used when creating playbooks. It works by connecting to your nodes and pushing out small programs.

## Provisioning

### App

In the ansible app.yml playbook, the sources list are updated and any of the packages available are upgraded. The playbook then installs nginx, git, nodejs and a package manager (pm2). ejs, mongoose and express are then installed, then posts page is seeded, nginx reverse proxy is added and nginx is restarted.

### Database

In the ansible db.yml playbook the mongo packages are added and then the specific mongo 3.2 is added. Mongodb is restarted and the mongod.conf is replaced with a bindIp of 0.0.0.0 and port 27017 and mongodb is restarted again.

## Requirements

Downloads required:
- download vagrant 2.2.6
- download oracle vm virtualbox 6.0.14

Ansible is installed in the vagrantfile when the virtual machines spin up so no need to install ansible on your local machine.

## Running the app

To start up the machine:
```
vagrant up
```
Enter the machine containing the app using:
```
vagrant ssh app
```
In the command line run the following code to enter into the app file:
```
cd /home/ubuntu/app
```
In the command line run the following code to start running the app:
```
node app.js
```
## Loading and using app

To see the app running go to: http://development.local:3000

For more information on using this app https://github.com/dilanmorar/node-sample-app
