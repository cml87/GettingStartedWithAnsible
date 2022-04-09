# Ansible

Ansible is an automation tool, similar to Chef, Puppet and SaltStack. In <b>enterprise environments</b>, it is used to:
1. Automatize pipelines in a reliable way.
2. Automatize configuration of any environment: VMs, containers, pods, etc.
3. Aid in build and deploys tools, with other tools such as Jenkins
4. Abstract the communication with all the environments it can work with: VMs, containers, etc

We can think of different degrees of automation: bash scripts, Ansible adhoc commands, Ansible playbooks.
A bash script is <b>imperative</b>, as we must explicitly specify what we want to do in order to achieve a <u>desired state</u>; we most specify the <i>how</i> and adapt it to the <u>initial state</u> we have. Ansible is <b>declarative</b>, as we only must specify the desired state. This let us focus in our goal, Ansible will take care of the <i>how</i>!





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
