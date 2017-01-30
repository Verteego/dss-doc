############
Installation
############

**Please note that in the following installation instructions we use 2 placeholders for local directories: VDS_ROOT and SSH_ROOT. Just replace them with the correct directories from your system.**

- VDS_ROOT: directory where Verteego Data Suite will be cloned
- SSH_ROOT: current user's .ssh directory


1. Install Ansible
""""""""""""""""""
You need to have Ansible installed on your local machine to manage the automatic installation process for you. If you don't have it yet, please install Ansible as we'll use it to deploy Verteego DS to your remote server or Virtualbox.

**Linux**

http://docs.ansible.com/ansible/intro_installation.html#latest-releases-via-apt-ubuntu

**Mac OS (Not tested)**

http://docs.ansible.com/ansible/intro_installation.html

**Windows (Not tested)**

http://docs.ansible.com/ansible/intro_installation.html


2. Clone installation package
"""""""""""""""""""""""""""""
Clone the following repository to your local machine (NOT to the remote server on which you want run Verteego DS). This repository will be called VDS_ROOT in the following.

::

    git clone git@github.com:Verteego/vds.git


3. Install Verteego DS
""""""""""""""""""""""

**INSTALLATION ON A LOCAL VIRTUAL SERVER (VIRTUALBOX)**

- Install Virtualbox : https://www.virtualbox.org/wiki/Downloads
- Install Vagrant    : https://www.vagrantup.com/docs/installation/
- Go to the Vagrant directory (VDS_ROOT/vagrant) and launch Vagrant (this may take some time as it will download a full Debian image to install on Virtualbox):

::

    cd VDS_ROOT/vagrant
    vagrant up

- Run the Ansible playbook

::

    ansible-playbook -i $VDS_ROOT/deployment/ansible/hosts --private-key=VDS_ROOT/vagrant/.vagrant/machines/dss/virtualbox/private_key $VDS_ROOT/setup_cluster.yml


- Navigate to http://VIRTUALBOX_INSTANCE_IP:33330



**INSTALLATION ON GOOGLE CLOUD PLATFORM**

**1. Install Google Cloud SDK**

You should have a running Google Cloud platform account and the SDK installed.

- Install GCloud SDK: https://cloud.google.com/sdk/docs/
- Configure your account and project

::

    gcloud init



- Generate SSH key for GCloud

::

    gcloud compute config-ssh


**2. Set up the VDS environment on Google Cloud**

- Create a Google service account :
    - Go to https://console.cloud.google.com/iam-admin/serviceaccounts
    - Select the project into which you want to create the VDS instance
    - Create a service account with project editor role
    - Check the "Furnish a new private key" option
    - Chose JSON key type

.. image:: http://verteego-dss-doc.readthedocs.io/en/latest/_static/images/step_01.jpeg
    :scale: 50%

.. image:: http://verteego-dss-doc.readthedocs.io/en/latest/_static/images/step_02.jpeg
    :scale: 50%

    - Copy the downloaded key file to VDS_ROOT/deployment/ansible/files and rename it to ansible.json

::

     cp Downloads/ORIGINAL_KEYFILE.json VDS_ROOT/deployment/ansible/files/ansible.json


**3. Install libcloud**

::

    sudo apt-get install python-pip
    sudo pip install -U apache-libcloud


**4. Launch playbook**

::

    ansible-playbook -i VDS_ROOT/deployment/ansible/hosts --private-key=SSH_ROOT/google_compute_engine VDS_ROOT/deployment/ansible/setup_gc_instance.yml


- Be patient, the deployment of all files can take a while depending on the capacity of the server you've chosen.
- Navigate to the newly created instance IP (port 33330). You can find it on on your Google Cloud Compute Engine console: http://GC_INSTANCE_IP:33330


3. Sign in
""""""""""

For your first sign in you can use the following credentials. For security reasons, remember to change them or delete the default user after your first login.
Username: vds-user
Password: verteego


4. Customize settings
"""""""""""""""""""""