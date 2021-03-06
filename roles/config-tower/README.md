#DOC

##Config Tower

Initial Tower configuration and objects creation (templates, inventories and credentials) 


##Requirements

None

##Role Variables

org_name: Default 
scm_branch: master 
scm_url: "{{ github_repo }}"
proj_name: "00_Homework Assignment"
user_name: admin 
password: r3dh4t1!
ssh_cred_name: "Connect_to_workstation"
user_cred_name: "cloud-user"
user_cred_path: "/root/.ssh/openstack.pem"
static_inventory_name: scm_inventory
instance_group_name: osp
workflow_template_name: cicd_workflow
job_template_name_osp_instances: "02_Provision QA Env"
osp_instances_playbook: site-osp-instances.yml
group_name: workstation
host_name: workstation-{{osp_GUID}}.{{osp_DOMAIN}}
job_template_name_3tier_app: "04_3tier app deployment on QA Env"
app_deploy_playbook: site-3tier-app.yml
vault_name: yum_repo_vault
job_template_name_osp_instances_delete: "Nuke QA Env"
osp_instances_delete_playbook: site-osp-delete.yml
job_template_smoke_osp: "05_Smoke test QA Env"
osp_smoke_playbook: site-smoke-osp.yml
job_template_aws_provision: "01_Provision Prod Env"
aws_provision_playbook: aws_provision.yml
ec2_dynamic_inventory: "Prod Three tier inventory"
aws_ssh_playbook: aws_creds.yml
opentlc_cred_name: "Opentlc key"
job_template_aws_ssh_keys: "08_Prod SSH keys three tier app"
ec2_inventory_source: "03_06_Prod Three tier inventory source"
aws_region_name: "{{ REGION_NAME }}"
tag_filters: "tag:instance_filter=three-tier-app-{{ EMAIL }}"
scm_inventory_source: "SCM three tier inventory source"
path_to_scm_inventory: scm_inventory
aws_read_keys: "AWS Access Key"
aws_status_playbook: aws_status_check.yml
job_template_aws_status_check: "07_Prod Check the status of AWS instances"
job_template_name_3tier_app_aws: "09_3 tier app on Prod"
aws_cred_name: "Creds for AWS instances"
job_template_smoke_aws: "10_Smoke test Prod env"
aws_smoke_app_playbook: site-smoketest-aws.yml

##Dependencies

None

##Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: localhost
      connection: local
      gather_facts: false
      become: false
      vars:
        tower_cli_conf_file_path: /root/.tower_cli.cfg
      roles:
        - name: config-tower
