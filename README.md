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
Plugins are divided in the following <u>categories</u> (as of 2020)
- <b>become</b>: Responsible for enabling Ansible to obtain super-user access (for example, through sudo). See  https://docs.ansible.com/ansible/latest/user_guide/become.html. Don't know how to use it.
- cache: Responsible for caching facts (like system properties) retrieved from backend systems to improve automation performance
- callback: Allows you to add new behaviors when responding to events —for example, changing the format data is printed out in the output of an Ansible
playbook run.
- cliconf: Provides abstractions to the command-line interfaces of various network devices, giving Ansible a standard interface to operate on.
- <b>connection</b>: Provides connectivity from Ansible to remote systems (for example, over SSH, WinRM, Docker, and many more)
- httpapi: Tells Ansible how to interact with a remote system's API (for example, for a Fortinet firewall)
- <b>inventory</b>: Provides Ansible with the ability to parse various static and dynamic inventory formats.
- lookup: Allows Ansible to look up data from an external source (for example, by reading a flat text file)
- <b>module</b>: another type of Ansible plugin ...
- netconf: Provides Ansible with abstractions to enable it to work with NETCONF-enabled networking devices.
- shell: Provides Ansible with the ability to work with various shells on different systems (for example, powershell on Windows versus sh on Linux)
- strategy: Provides plugins to Ansible with different execution strategies (for example, the debug strategy, Playbooks and Roles)
- vars: Provides Ansible with the ability to source variables from certain sources, such as the host_vars and group_vars directories,
Defining Your Inventory)

We can list all the installed plugins under one category with
```shell
$ ansible-doc -t connection -l  // list all plugins of type 'connection'
[WARNING]: Collection ibm.qradar does not support Ansible version 2.12.2
[WARNING]: Collection splunk.es does not support Ansible version 2.12.2
[WARNING]: Collection frr.frr does not support Ansible version 2.12.2
ansible.netcommon.httpapi      Use httpapi to run command on network appliances                                                                                                          
ansible.netcommon.libssh       (Tech preview) Run tasks using libssh for ssh connection                                                                                                  
ansible.netcommon.napalm       Provides persistent connection using NAPALM                                                                                                               
ansible.netcommon.netconf      Provides a persistent connection using the netconf protocol                                                                                               
ansible.netcommon.network_cli  Use network_cli to run command on network appliances                                                                                                      
ansible.netcommon.persistent   Use a persistent unix socket for connection                                                                                                               
community.aws.aws_ssm          execute via AWS Systems Manager 
...
```
We can get help about an specific plugin in one category with
```shell
$ ansible-doc -t connections ssh // get help about the connection plugin 'ssh'
```

## Ansible adhoc command
The example:
```shell
$ ansible -m copy -a "src=master.gitconfig dest=~/.gitconfig" [--check] [--diff] localhost
```
is an example of Ansible adhoc command using the Ansible module <code>copy</code>. One of the most common options used with it are:

 -C, --check &emsp;         dry-run. Don't make any changes; instead, try to predict some of the changes that may occur  
 -D, --diff  &emsp;        when changing (small) files and templates, show the differences in those files; works great with --check
-v, -vv, -vvv  &emsp;  use for different levels of verbosity

## Ansible modules
A module is a type of plugin. Ansible modules are well made and tested Python scripts designed to accomplish a set of operations in a given domain-of-things, for example the <code>copy</code> module. Modules execute in the managed nodes:
```shell
$ ansible-doc -t module -l
$ ansible-doc copy  //equivalent to "ansible-doc -t module copy", get help about the 'copy' module
```

With the Ansible adhoc command we can use the <code>copy</code> module as
```shell
$ ansible -m copy -a "src=master.gitconfig dest=~/.gitconfig" localhost
$ ansible -m homebrew -a "name=bat state=latest" localhost // install the "bat" command, latest version in localhost (desired stated)
```
Here we target "localhost".

In the example above the <u>desired state</u> is to have the specific source file in the specific destination file. Ansible will achieve this desired state whatever the <u>initial conditions</u> are:
- destination file exists and is identical (sha1) -> nothing will be done
- destination file exists but is different -> the source file will be copied to the destination file
- destination file does not exits -> the source file will be copied to the destination file

The <code>copy</code> module is an example of <u>idenpotent</u> module. It will give the same result regardless the number of re-runs.
Not all Ansible modules are idenpotent. For example, <code>comand</code> and <code>debug</code> are not; they don't ensure any specific state, they just show something.

Ansible modules are normally called inside task of Ansible <u>playbooks</u>.


# Ansible playbooks
Ansible modules are meant to be used in Ansible playbooks, not in Ansible adhoc commands. Ansible adhoc commands are more inefficient, as each time one is executed it's gathered information about the system, for example. In a playbook the information is only gathered once.

```bash
# mySimplePlaybook.yml
- name: Play number 1  ## a play
  hosts: localhost  ## ?? parameter
  tasks:                              ## list of tasks
  - name: run the copy module         ## name of the task
    copy: src="~/t1/file.txt" dest="~/t2"  ## module or action

  - name: run the copy module again
    copy:                           ## module or action?
      src: "~/t1/file.txt"       ## arg
      dest: "~/t2"               ## arg
      follow: yes                ## arg

- name: Play number 2
  hosts: localhost
  gather_facts: false  ## ?? parameter
  tasks:  
    - name: git_config module simplifies listing configuration 
      git_config: list_all=yes scope=global
```

In general a playbook has a set of plays, each with a name, and each play has a 'tasks' field. Under the tasks field we specify different named tasks (or actions). A task can be a call to a module, buy may be something else, it seems. We execute a playbook with the <code>ansible-playbook</code> command:
```shell
$ ansible-playbook mySimplePlaybook.yml [-v, -vvv, --diff, --check]
```

Each play in a playbook will run the "Gathering Facts" task by default as the first one, which gather info about all the nodes Ansible has discovered at the time the playbook is run. This task runs the <code>setup</code> module, (<code>$ ansible-doc setup</code>). To disable it use the parameter <code>gather_facts: false</code> at play level.

```shell
$ ansible -m setup localhost
$ ansible all -m ansible.builtin.setup --tree /tmp/facts # Display facts from all hosts and store them indexed by I(hostname) at C(/tmp/facts).
```

Task of a play may need these facts, so we may encounter some problems if we disable it.

Ansible playbooks must follow strict yml formatting rules. See yaml.org


## Ansible galaxy



___________________
___________________


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
