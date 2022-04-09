# Ansible
These are my Ansible notes from the course <span style="color:aquamarine">Getting started with Ansible</span>, from Pluralsight.

I will also take info from:
<span style="color:aquamarine">Practical Ansible 2, Automate infrastructure, manage configuration, and deploy applications with Ansible 2.9</span>
, by Daniel Oh, James Freeman and Fabio Alessandro Locati.


Ansible is an automation tool, similar to Chef, Puppet and SaltStack. In <b>enterprise environments</b>, it is used to:
1. Automatize pipelines in a reliable way.
2. Automatize configuration of any environment: VMs, containers, pods, etc.
3. Aid in build and deploys tools, with other tools such as Jenkins
4. Abstract the communication with all the environments it can work with: VMs, containers, etc

Ansible can communicate with and manage the following type of nodes, or environments, which normally are remote:
1. VMs, Linux and Windows type
2. bare metal servers, Linux and Windows
3. containers, docker
3. pods, Kubernetes

To work with this environments, Ansible use the so called <i>plugins</i> and <i>modules</i>, which are very well written and tested Python scripts, or applications. Plugins run in the control node, while modules run in the managed nodes.

We can think of different degrees of automation: bash scripts, Ansible adhoc commands, Ansible playbooks.

A bash script is <b>imperative</b>, as we must explicitly specify what we want to do in order to achieve a <u>desired state</u>; we most specify the <i>how</i> and adapt it to the <u>initial state</u> we have. 

Ansible is <b>declarative</b>, as we only must specify the desired state. This let us focus in our goal, Ansible will take care of the <i>how</i> !

## Install Ansible 
Ansible is a <b>Python</b> app. We install it with
```shell
$ pip list --outdated
$ pip install -U ansible
$ ansible --version
ansible [core 2.12.2]
  config file = None
  configured module search path = ['/home/camilo/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /home/camilo/.local/lib/python3.8/site-packages/ansible
  ansible collection location = /home/camilo/.ansible/collections:/usr/share/ansible/collections
  executable location = /home/camilo/.local/bin/ansible
  python version = 3.8.10 (default, Mar 15 2022, 12:22:08) [GCC 9.4.0]
  jinja version = 3.0.3
  libyaml = True
```
We can see all the different <u>Ansible commands</u> we have installed. All of them support the <code>-h</code> option. 
```shell
$ ansible //+ \tab
ansible             ansible-connection  ansible-doc         ansible-inventory   ansible-pull        ansible-vault       
ansible-config      ansible-console     ansible-galaxy      ansible-playbook    ansible-test
$ ansible -h
usage: ansible [-h] [--version] ...
...
Define and run a single task 'playbook' against a set of hosts

positional arguments:
  pattern               host pattern
...
```

## Ansible architecture
Ansible has a centralized architecture. Only the central node must have Ansible installed. All nodes, central and managed, must have a Python interpreter installed, and ansible must know where it is.
![image info](./pictures/ansible_architecture.jpg)


## Ansible plugins
Ansible plugins augment Ansible's core functionalities. Plugins run in the control node within the Ansible process.
Plugins are divided in the following categories (as of )
- become: Responsible for enabling Ansible to obtain super-user access (for example, through sudo)
- cache: Responsible for caching facts (like system properties) retrieved from backend systems to improve automation performance
- callback: Allows you to add new behaviors when responding to events —for example, changing the format data is printed out in the output of an Ansible
playbook run.
- cliconf: Provides abstractions to the command-line interfaces of various network devices, giving Ansible a standard interface to operate on.
- connection: Provides connectivity from Ansible to remote systems (for example, over SSH, WinRM, Docker, and many more)
- httpapi: Tells Ansible how to interact with a remote system's API (for example, for a Fortinet firewall)
- inventory: Provides Ansible with the ability to parse various static and dynamic inventory formats.

- lookup: Allows Ansible to look up data from an external source (for example, by reading a flat text file)
- netconf: Provides Ansible with abstractions to enable it to work with NETCONF-enabled networking devices.
- shell: Provides Ansible with the ability to work with various shells on different systems (for example, powershell on Windows versus sh on Linux)
- strategy: Provides plugins to Ansible with different execution strategies (for example, the debug strategy, Playbooks and Roles)
- vars: Provides Ansible with the ability to source variables from certain sources, such as the host_vars and group_vars directories,
Defining Your Inventory)


## Ansible modules
Ansible modules are well made and tested Python scripts. Modules execute in the managed nodes.

ljkjkjlkj





## Ansible modules

## Ansible galaxy





## Spin up Ansible multi VM lab with Vagrant

- Vagrant makes it easy to spin up VMs (in this case using virtualbox and pre-built vagrant boxes for ubuntu and centos)

## Instructions for following along

- Install
  - Vagrant: <https://www.vagrantup.com/docs/installation/>
  - VirtualBox: <https://www.virtualbox.org/wiki/Downloads>
- Create VMs
  - `cd` into this folder
  - `vagrant up` creates all VMs
    - or `vagrant up ubuntu10` to create one of the VMs
    - or pass other patterns to `vagrant up` to create other subsets of the VMs

## Notes

- As time passes you may want to update the OS versions and respective vagrant boxes in the `Vagrantfile`
  - Or, change vagrant boxes to simulate a different environment (ie OS) and experiment with what you can do with Ansible in that environment.
  - Vagrant box search: <https://app.vagrantup.com/boxes/search>
- Vagrant docs: <https://www.vagrantup.com/docs/>
- Vagrant provisioners for Ansible:
  - Vagrant provides two Ansible provisioners that are alternatives  to the way I demo Ansible in the course. Once you're comfortable with Ansible, these are a nice way to combine the two tools in a way that declaratively describes what Vagrant should have Ansible provision.
  - Run `ansible-playbook` on VM host: <https://www.vagrantup.com/docs/provisioning/ansible.html>
  - Run `ansible-playbook` on guest VM: <https://www.vagrantup.com/docs/provisioning/ansible_local.html>
