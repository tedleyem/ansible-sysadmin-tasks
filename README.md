# Ansible Sysadmin Taks 

A list of sysadmin tasks to use for Ansible Tower (automation platform)
or with ansible cli. This list has and will grow over time. 

### Structure
```

.
└── inventory <-- local inventory to use for testing these playbooks and roles 
├── playbooks <-- the directory containing the sysadmin tasks 
│   ├── security <-- the directory where you can find a few exploit checks 
├── README.md
├── roles <-- A few roles that can be used for mixed systems
└── test <-- Dockerfiles for Ansible test provisioning with Vagrant
└── vars
    └── main.yml
└── Vagrantfile <-- Vagrantfile used for testing playbooks

    ```
 
---
## Setup

##### Create new role
 To create a new role you can run the following commands 
```
$ ansible-galaxy role init roles/<role-name>
```
To remove ansbile-galaxy meta tags 
```
$ rm -rf <role-name>/meta
```

## Vagrant Local Development
To test these out locally you can run the Vagrantfile 
in the root of this project. 

The Vagrantfile uses [docker](https://developer.hashicorp.com/vagrant/docs/providers/docker/basics) as the provider.
Once installed with virtualbox set the provider and run. 
```
$ vagrant up --provider=docker
```




