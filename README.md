Profilazione HW ABO-01
Martedì, 21 settembre · 11:00AM – 12:00PM
Informazioni per partecipare di Google Meet
Link alla videochiamata: https://meet.google.com/sjh-nqoq-tkj
Oppure digita: ‪(IT) +39 02 3046 1412‬ PIN: ‪447 354 171‬#
Altri numeri di telefono: https://tel.meet/sjh-nqoq-tkj?pin=6405942808651
Incontro automazione VPN
Martedì, 21 settembre · 10:00 – 11:00AM
Informazioni per partecipare di Google Meet
Link alla videochiamata: https://meet.google.com/kjk-tczb-rra
Oppure digita: ‪(IT) +39 02 8732 3545‬ PIN: ‪787 688 193‬#
Altri numeri di telefono: https://tel.meet/kjk-tczb-rra?pin=6248015710438
https://gitlab-bper.gbbper.priv/Sistemisti/ansible-playbook-metricbeat/blob/master/README.md
<h1> ansible-playbook-Metricbeat <h1>

<h2> Automation by Opportunity ABO25 <h2>

The goal of this workflow is to install and configure the specific version 7.12.1 of Metricbeat, and after these operations start the Metricbeat service on the target machines.
The activities that must be performed are:
>
> - Identify the number of machines where the product will be installed
> 
> - The next step is the installation of Metricbeat
> 
> - Apply the values ​​to be modified from the template
>
> - Start the service


<h3> Hosts <h3>

Ansible works against multiple managed nodes or “hosts” . In this playbook the value added is that we can manage different OS at the same time using a list or group of lists known as inventory. Once your inventory is defined, you use patterns to select the hosts or groups you want Ansible to run against

<h3> Execution <h3>

The playbook will be run with this two commands , the first one is for execute with a passphrase and apply all the tasks to reach the goal described above , the other one will not make any changes on remote system:



<h4> Memorize the passhprase in the current session only in playbook needed <h4>

**eval `ssh-agent`**

**ssh-add /repository/key/id_rsa_ansible-t-2**

---- example stdout
[u86587@b05387.adgruppo.priv@ansible-t-01 ansible-playbook-metricbeat]$ **eval `ssh-agent`**


Agent pid 12614



[u86587@b05387.adgruppo.priv@ansible-t-01 ansible-playbook-metricbeat]$ **ssh-add /repository/key/id_rsa_ansible-t-2**


Enter passphrase for /repository/key/id_rsa_ansible-t-2:
Identity added: /repository/key/id_rsa_ansible-t-2 (/repository/key/id_rsa_ansible-t-2)

-----
The activities carried out during the run management will be logged and stored in a log file that must be saved in a predefined area with timestamp:


<h2> Complete step to apply <h2>
<h4> Setting correct log format <h4>

 **export ANSIBLE_LOG_PATH=/repository/log/ansible-MetricBeat.`date +%s`.log**

**ansible-playbook metricbeat.yml -i hosts --vault-id automation-credential@prompt**


__ansible-playbook metricbeat.yml -i hosts --vault-id automation-credential@prompt -e "elk_document_type=^pushgw-*  elk_ufficio_app_owner=ufficio_enterprise_applications_e_design" __


in this example we specify the:

**app owner ufficio_enterprise_applications_e_design**

**the document type ^pushgw-***



Now let's understand more in deep what the playbook *does to play*.

Based on the requirements collected so far, the proposed solution is composed of the following Steps:
The first task will *install Metricbeat* on the target machines, which can be Windows or Linux for the playbook is the same . It check the variables takes from input and then makes the things done.


#variable that you can use to modify the execution

**elk_ufficio_app_owner**********--> this is the string to identify the app owner of the machine (see the table to see the value) 

example
elk_ufficio_app_owner: "ufficio_enterprise_applications_e_design"

******elk_document_type******** --> this is the string to identify the document type (see the table to see the value) 

example

elk_document_type: "^pushgw-*"


****kafka_hosts**--> this i s the list of destination kafka or elasticsearch node

example and default of this node
kafka_hosts: '"elk7-elasticsearch-coll01.gbbper.priv:9200","elk7-elasticsearch-coll02.gbbper.priv:9200","elk7-elasticsearch-coll03.gbbper.priv:9200"'




Table of value:  **elk_document_type elk_ufficio_app_owner: 




| Document_type      | elk_app_owner_ufficio |
| -----------  | ----------- |
| ^pushgw-*      |             ufficio_enterprise_applications_e_design |
| ^servicegw-*   |          ufficio_enterprise_applications_e_design    |
| ^trasparenza-* |       ufficio_sistemi_canali_diretti_e_mobility      |
| ^webrainbow-*  |      ufficio_enterprise_applications_e_design        |
| ^smart-sca*    |        ufficio_sicurezza_operativa_ict               |    
|^aree-riservate-* |     ufficio_sistemi_canali_diretti_e_mobility       |  




The second task *applies the values* ​​which have to be changed from the template, in this case the template is in the folder templates, it has two different configuration and it differs from the OS.
**In this template we left the original output part cause kafka output is not already prepared.**

The last step will *start the Metricbeat service*. This last tasks first enables the services and then start it automatically.

<h2> Step for checking <h2>

Ansible provides two modes of execution that validate tasks: check mode and diff mode.

These modes can be used separately or together. They are useful when you are creating or editing a playbook or role and you want to know what it will do.

In check mode, Ansible runs without making any changes on remote systems. 
**ansible-playbook metricbeat.yml -i hosts - --check**

<h3> Ansible galaxy and requirements.yml <h3>

Use the ansible-galaxy command to download roles from the Galaxy server. 
Multiple roles can be installed by listing them in a requirements.yml file. The format of the file is YAML, and the file extension must be either .yml or .yaml.

Use the following command to install roles included in requirements.yml(need access to https://gitlab-bper.gbbper.priv/ and all the repository in the file requirements.yml ): 

**ansible-galaxy install -r requirements.yml**

Each role in the file will have one or more of the following attributes:
>
> - src
The source of the role, and a required attribute. Specify a role from Galaxy by using the format namespace.role_name, or provide a URL to a repository within a git based SCM.
>
> - scm
If the src is a URL, specify the SCM. Only git or hg are supported. 
>
> - version 
The version of the role to download. Provide a release tag value, commit hash, or branch name. Defaults to the branch set as a default in the repository, otherwise defaults to the master.

If there is some new version of the role to update, we have to run this command : 
**ansible-galaxy install -r requirements.yml --force**
