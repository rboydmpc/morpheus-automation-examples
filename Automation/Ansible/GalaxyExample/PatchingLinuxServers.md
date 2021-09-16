# Patching Linux servers using Anisble with Morpheus to report on the Patching data.

### Use Case:
When a user deploys an instance *in this use case we are referring to Centos 7* from Morpheus, based on the Operating system and version the instance should be added to a patching job. The job is scheduled to run on the 29th of every month at 23:45hrs. The job runs an ansible playbook on all the servers attached, the play patching the OS to latest verion. The patching results (success, failed, noChanges) are then recorded in Morpheus. A custom report type (report template) is added to generate a report on the patching results.

This use case is an example of the level of customization and User friendly UI with RBAC that can be done.

![alt text](https://github.com/gomorpheus/morpheus-automation-examples/blob/main/src/common/images/Patching_new.png "Flow")

<dd>The above image shows the flow of user self service provisioning. User submits the instance provisioning of a Centos VM via Morpheus UI/API. The 
 
**[Video](https://www.youtube.com/watch?v=iLDZZVEkkos)**

## Pre-reqs:
Morpheus v5.3.2 +
[Download](https://morpheushub.com/download)

[Install Ansible](https://docs.morpheusdata.com/en/latest/integration_guides/Automation/ansible.html#id1) on Morpheus App server(s)
Ubuntu
```
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```
Centos
```
sudo yum install epel-release
sudo yum install ansible
```
Then create the working Ansible directory for Morpheus
```
sudo mkdir /opt/morpheus/.local/.ansible
sudo chown -R morpheus-app.morpheus-local /opt/morpheus/.local/.ansible
```

[Install Python on Morpheus App Server(s)]()

## Install galaxy collection 
Install [mysql](https://docs.ansible.com/ansible/latest/collections/community/mysql/mysql_query_module.html) collection

Install [patching](https://galaxy.ansible.com/ataha/linux_patching) collection

Run the below commands on morpheus app server(s) as root:
```
ansible-galaxy collection install ataha.linux_patching
ansible-galaxy collection install community.mysql
mv /root/.ansible/collections/ansible_collections/ataha /opt/morpheus/.local/.ansible/collections/ansible_collections/
mv /root/.ansible/collections/ansible_collections/community /opt/morpheus/.local/.ansible/collections/ansible_collections/community/
chown -R morpheus-app.morpheus-local /opt/morpheus/.local/.ansible/collections/ansible_collections/
```

## Add Ansible Integration in morpheus
[How to Doc](https://docs.morpheusdata.com/en/latest/integration_guides/Automation/ansible.html#add-ansible-integration)

<dl>
<b>Use the below values for the integration</b>
<dt>Ansible Git URL</dt> 
<dd>https://github.com/cuxtud/morpheus-ansible2.git</dd>

<dt>Default Branch</dt> 
<dd>master</dd>

<dt>Playbooks Path</dt> 
<dd>/</dd>

<dt>Roles Path</dt> 
<dd>/roles</dd>

<dt>Group Variables Path</dt> 
<dd>/group_vars</dd>

<dt>Host Variables Path</dt> 
<dd>/hosts</dd>

<dt>User MorpheusAgent Command Bus</dt> 
<dd>checked</dd>
</dl>

![alt text](https://github.com/gomorpheus/morpheus-automation-examples/blob/main/src/common/images/AddAnsibleIntegration.png "Ansible Integration")

<dt> Save Changes </dt>

## Add git integration 

*Add in Morpheus UI. Nagivate to Administration > Integration > Add -> Git*

<dt> Name </dt>
<dd> Enter any name </dd>

<dt> Git Url </dt>
<dd> https://github.com/gomorpheus/morpheus-automation-examples </dd>

<dt> Default Branch </dt>
<dd> main </dd>

![alt text](https://github.com/gomorpheus/morpheus-automation-examples/blob/main/src/common/images/gitIntegration.png "Git Integration")

<dt> Save Changes </dt>

## Add a task of type Ansible and refer the playbook

*In Morpheus UI, Browse to Provisioning > Automation > Tasks > Add*

<dt> Name </dt>
<dd> <i> Enter a Name </i> </dd>

<dt> Type </dt>
<dd> Select Ansible Playbook </dd>

<dt> Ansible Repo </dt>
<dd> Select the ansible integration from the list </dd>

<dt> Playbook </dt>
<dd> patching_linux.yml </dd>

<dt> Execute Target </dt>
<dd> Resource </dd>

![alt text](https://github.com/gomorpheus/morpheus-automation-examples/blob/main/src/common/images/AnsibleTask.png "Ansible Task")

<dt> Save Changes </dt>

## Add a task of type python and refer to the updateJob.py script from git source. 

<dt> Name </dt>
<dd> <i> Enter a name </i> </dd>

<dt> Type </dt>
<dd> Select Python Script </dd>

<dt> Result Type </dt>
<dd> None </dd>

<dt> Source </dt>
<dd> Select Repository </dt>

<dt> Repository </dt>
<dd> Select the git integration </dd>

<dt> File Path </dt>
<dd> Automation/Jobs/updateJob.py </dd>

<dt> Additional Packages </dt>
<dd> requests simplejson </dd>

<dt> Execute Target </dt>
<dd> Local </dd>

![alt text](https://github.com/gomorpheus/morpheus-automation-examples/blob/main/src/common/images/pythonTask.png "Python Task")

<dt> Save Changes </dt>

## Add the python task to a provisioning workflow.

## Add a custome instance type of Centos and attach the workflow to the layout

## Create a execution schedule in Morpheus 

## Create a Job in Morpheus with associated python Task to updateJob

## Add a cypher in morpheus of mount secret to store the DB password

## Create patching table in morpheus DB

## Upload the patching report plugin in Morpheus

## Provision an instance 

## Execute the Ansible task to patch the Centos instance deployed

## Run the ansible task on the instance

## Generate Report