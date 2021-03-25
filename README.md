# Ansible Advanced Homework

## Description
Repo with information about final LAB for the Red Hat Delivery Specialist—Ansible Advanced accreditation.
Required next OPENTLC lab environments:
- Ansible Advanced - Homework
- Ansible Advanced NG - OpenStack
- Ansible Advanced - Three Tier App

Note that "Ansible Advanced - Three Tier App" environment is provisioned as a task during lab execution, so is not needed to deploy manually.


## Environment preparation
Login as root in the control node in the "Homework" environment.
Clone this repo.
Setup environment variables:

```bash
[root@control ~/nextgen_ansible_advanced_homework]# cat ~/labrc
export TOWER_GUID=<Tower env GUID>
export OSP_GUID=<OSP Env GUID>
export OSP_DOMAIN=<OSP Env domain name>
export OPENTLC_ID=<username-example.com>
export MAIL_ID=<username@example.com>
export OPENTLC_PASSWORD=<Your OPENTLC Password>
export GITHUB_REPO=https://github.com/<github username>/nextgen_ansible_advanced_homework.git
export JQ_REPO_BASE=http://www.opentlc.com/download/ansible_bootcamp
export REGION=us-east-1
```

Set the env vars from ~/labrc

```bash
[root@control ~]# source ~/labrc 
```

Setup the prerequisites for the control node
```bash
mv ~/ansible-tower-setup-*/ ~/ansible-tower-setup-latest
cp /etc/ansible/hosts ~/ansible-tower-setup-latest/inventory
chmod 0400 /root/.ssh/openstack.pem
ansible-playbook site-setup-prereqs.yaml -k
```

Tower admin password must be set to r3dh4t1! in the tower console and in the tower-cli config file
```bash
[root@control ~]# cat ~/.tower_cli.cfg
[general]
host = tower1.<guid>.internal
username = admin
password = r3dh4t1!
verify_ssl = False
```

## Tower environment setup
Run site-config-tower.yml in the control node.

```bash
[root@control ~/nextgen_ansible_advanced_homework]# ansible-playbook site-config-tower.yml -e tower_GUID=${TOWER_GUID} -e osp_GUID=${OSP_GUID} -e osp_DOMAIN=${OSP_DOMAIN} -e opentlc_login=${OPENTLC_ID} -e path_to_opentlc_key=/root/.ssh/mykey.pem -e param_repo_base=${JQ_REPO_BASE} -e opentlc_password=${OPENTLC_PASSWORD} -e REGION_NAME=${REGION} -e EMAIL=${MAIL_ID} -e github_repo=${GITHUB_REPO}
```

## QA and PRO environment deployment
The environments are deployed using the cicd_workflow template in Tower console.
Once the template is launched all the infrastructure is deployed in RHOSP (QA) and AWS (PRO) and the Smoke tests are passed.
 

## Tower templates

Next templates are presents in Tower enviroment:

| Job Template                              | Playbook name          | Description                                                                                                       |
|-------------------------------------------|------------------------|---------------------------------------------------------------------------------------------------------------|
| 01_Provision Prod Env                     | aws_provision.yml      | Provision production environment in AWS                                                                       |
| 02_Provision QA Env                       | site-osp-instances.yml | Provision QA environment in OpenStack                                                                         |
| 04_3tier app deployment on QA Env         | site-3tier-app.yml     | Deploy three tier application to QA                                                                           |
| 05_Smoke test QA Env                      | site-smoke-osp.yml     | Playbook to test three tier app on OSP                                                                        |
| 07_Prod Check the status of AWS instances | aws_status_check.yml   | Check aws instances are up or not                                                                             |
| 08_Prod SSH keys three tier app           | aws_creds.yml          | Use ssh private key from AWS bastion host and use it to create machine credential to connect to AWS instances |
| 09_3 tier app on Prod                     | site-3tier-app.yml     | Deploy three tier application to Production                                                                   |
| 10_Smoke test Prod env                    | site-smoketest-aws.yml | test three tier app on AWS                                                                                    |
| Nuke QA Env                               | site-osp-delete.yml    | Delete QA OpenStack instances 


Executed in control node (no template associated):

| Playbook name          | Purpose                                                                                                       |
|------------------------|---------------------------------------------------------------------------------------------------------------|
| site-config-tower.yml  | Configure Ansible Tower with jobs from this project                                                           |
| site-setup-prereqs.yml | Configure Ansible Tower to use isolated node                                                                  |
| grading-script.yml     | Self grading script

## Roles

See documentation in roles folders

| Playbook name       | Description                                                              |
|---------------------|--------------------------------------------------------------------------|
| app-tier            | Install application on server                                            |
| db-tier             | Install postgresql server                                                |
| lb-tier             | Install HA proxy server                                                  |
| base-config         | Setup yum repo and base packages role                                    |
| setup-workstation   | Setup workstation, create network, ssh keypair, security group etc. role |
| osp-servers         | Provision OSP Instances role                                             |
| osp-instance-delete | Delete OSP Instances role                                                |
| osp-facts           | Generate in-memory inventory for OSP instances role                      |
| config-tower        | Role to configure ansible tower job templates and workflow               |

## Notes
- In the template "08_Prod SSH keys three tier app" there is an error with the connection to production bastion instance as there is a problem with the connection with my personal key. It doesn't work in the previous labs and can not be solved for the Final lab, For check the correct execution of the code I've copied manually the keys to bastion and executed again, working fine.

- In the template "10_Smoke test Prod env" I had an error because the cfg file used in lb-tier role (haproxy.cfg.j2) is not prepared for be used in QA environment and AWS environment. The var "hostvars[host]['inventory_hostname']" is returning the IP address for the in-memory inventory used for QA env and the host name for the dinamic inventory used for AWS. Two solutions could be used:
	- Duplicate the role for the loadbalancer configuration keeping as is for QA environment and changing the value in the cfg file for the role used for AWS (change {{hostvars[host]['inventory_hostname']}} by {{hostvars[host]['private_ip_address']}}).
	- Duplicate just the j2 file in the templates folder with the changes mentioned before and use a conditional execution in the tasks file to use each file depending the inventory used.
- About the grading playbook, two remarks:
	- The task Access Website for the QA environment has been corrected as the item dict fact that is being compared with "frontend" must be name instead of hostname. changing it the test pass.
	- For the Prod environment the problem is the same that running the workflow, the config file must be edited to configure the lb with the value under private_ip_address. Making the propper changes and running again, it works.


