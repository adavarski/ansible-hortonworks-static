Manual install:

Env Ubuntu 18.04:
```
sudo apt-get update
sudo apt-get -y install unzip python-virtualenv python-pip python-dev sshpass git libffi-dev libssl-dev libyaml-dev vim

Install required packages:

sudo apt-get update
sudo apt-get -y install unzip python-virtualenv python-pip python-dev sshpass git libffi-dev libssl-dev libyaml-dev vim
Create and source the Python virtual environment

virtualenv ~/ansible; source ~/ansible/bin/activate
Install the required Python packages inside the virtualenv

pip install setuptools --upgrade
pip install pip --upgrade
pip install ansible

git clone https://github.com/adavarski/ansible-hortonworks

cd  ./ansible-hortonworks

export CLOUD_TO_USE=static
. set_cloud.sh 
The static inventory will be used.
davar@home ~/LABS/ansible-hortonworks $ export EXTRA_VARS_HDP=@./hdp-cluster-minimal.yml
davar@home ~/LABS/ansible-hortonworks $ export STATIC_INI=./inventory/static
davar@home ~/LABS/ansible-hortonworks $ 



ansible-playbook -v -e "cloud_name=${cloud_to_use}" playbooks/prepare_nodes.yml --inventory="$STATIC_INI" --extra-vars="$EXTRA_VARS_HDP"

ansible-playbook -v -e "cloud_name=${cloud_to_use}" playbooks/install_ambari.yml --inventory="$STATIC_INI" --extra-vars="$EXTRA_VARS_HDP" 

ansible-playbook -v -e "cloud_name=${cloud_to_use}" playbooks/configure_ambari.yml --inventory="$STATIC_INI" --extra-vars="$EXTRA_VARS_HDP"

ansible-playbook -v -e "cloud_name=${cloud_to_use}" playbooks/apply_blueprint.yml --inventory="$STATIC_INI" --extra-vars="$EXTRA_VARS_HDP"


TASK [ambari-blueprint : Upload the blueprint hdp-minimal_blueprint to the Ambari server] ********************************************************************
ok: [hadoopmaster] => {"cache_control": "no-store", "changed": false, "connection": "close", "content_type": "text/plain", "cookies": {"AMBARISESSIONID": "18py4rydkg1o81uera47a6zcq6"}, "cookies_string": "AMBARISESSIONID=18py4rydkg1o81uera47a6zcq6", "expires": "Thu, 01 Jan 1970 00:00:00 GMT", "msg": "OK (unknown bytes)", "pragma": "no-cache", "redirected": false, "set_cookie": "AMBARISESSIONID=18py4rydkg1o81uera47a6zcq6;Path=/;HttpOnly", "status": 201, "url": "http://hadoopmaster:8080/api/v1/blueprints/hdp-minimal_blueprint", "user": "VALUE_SPECIFIED_IN_NO_LOG_PARAMETER", "x_content_type_options": "nosniff", "x_frame_options": "DENY", "x_xss_protection": "1; mode=block"}

TASK [ambari-blueprint : Launch the create cluster request] **************************************************************************************************
ok: [hadoopmaster] => {"cache_control": "no-store", "changed": false, "connection": "close", "content": "{\n  \"href\" : \"http://hadoopmaster:8080/api/v1/clusters/hdp-minimal/requests/1\",\n  \"Requests\" : {\n    \"id\" : 1,\n    \"status\" : \"Accepted\"\n  }\n}", "content_type": "text/plain", "cookies": {"AMBARISESSIONID": "18mwmwxjmgc6y1p26u2gmo9v1e"}, "cookies_string": "AMBARISESSIONID=18mwmwxjmgc6y1p26u2gmo9v1e", "expires": "Thu, 01 Jan 1970 00:00:00 GMT", "json": {"Requests": {"id": 1, "status": "Accepted"}, "href": "http://hadoopmaster:8080/api/v1/clusters/hdp-minimal/requests/1"}, "msg": "OK (unknown bytes)", "pragma": "no-cache", "redirected": false, "set_cookie": "AMBARISESSIONID=18mwmwxjmgc6y1p26u2gmo9v1e;Path=/;HttpOnly", "status": 202, "url": "http://hadoopmaster:8080/api/v1/clusters/hdp-minimal", "user": "VALUE_SPECIFIED_IN_NO_LOG_PARAMETER", "vary": "Accept-Encoding, User-Agent", "x_content_type_options": "nosniff", "x_frame_options": "DENY", "x_xss_protection": "1; mode=block"}

TASK [ambari-blueprint : Wait for the cluster to be built] ***************************************************************************************************
FAILED - RETRYING: Wait for the cluster to be built (360 retries left).
FAILED - RETRYING: Wait for the cluster to be built (359 retries left).



```

# ansible-hortonworks-configuration

## Motivation
The idea is to clone ansible-hortonworks from GitHub and change the variables without actually changing any of the files cloned.

### Introduction
The repository shows how to override variables in ansible-hortonworks/inventory/aws/group_vars/all and ansible-hortonworks/playbooks/group_vars/all using parameter extra-vars from the command line when running playbooks.

If ran out-of-the-box, the cluster - one namenode with Ambari and one datanode - is build in region Ireland (eu-west-1) with centos7 and the instance types are t2.xlarge.

### secret key and access key
In /etc/profile.d/sh.local add secret key and access key
export AWS_SECRET_ACCESS_KEY=
export AWS_ACCESS_KEY_ID=
(or some other way)

### Clone ansible-hortonworks repository
git-clone folder has a playbook that clones https://github.com/hortonworks/ansible-hortonworks.git

### Files in config folder:
- aws-cluster.yml - in eu-west-1, 2 instances are created of type t2.xlarge. Ami is centos 7.
- hdp-cluster-hadoop3.yml - creates an HDP 3.0 cluster based on the variables in this file
- hdp-cluster-minimal.yml - creates an HDP 2.6.5 (default by HDP in GitHub) with minimal services

### Example of how to run
. run-playbooks-with-extra-vars.sh config/hdp-cluster-hadoop3.yml

### Output
All hortonworks playbooks are ran with verbose -v and output directory is created at the start of the script. The name of the directory matches the name of the yml file used in extra-vars for HDP variables.
