# Purpose

A base setup for creating a running virtual machine using [Vagrant](https://www.vagrantup.com/) and provisioning [Sonatype Nexus](http://www.sonatype.org/nexus/) on the VM using [Ansible](http://docs.ansible.com/index.html).

# Getting Started (first-time configuration)

1. [Install VirtualBox]()
1. [Install Vagrant](https://docs.vagrantup.com/v2/installation/index.html)
1. [Install Ansible](http://docs.ansible.com/intro_installation.html#installation) (I recommend [Homebrew](http://brew.sh/) for OS X)
1. Open a terminal
    - `git clone https://github.com/gitlevich/JenkinsHostSetup`
    - `cd nexus-vagrant-ansible`
    - `vagrant up`
    - `vagrant ssh`
    - `sudo -u jenkins -i`
    - prepare Jenkins for configuration restore
        - create a backup directory  
        `mkdir /var/lib/jenkins/backup`  
        - copy our version-controlled backup file for further restoring Jenkins configuration  
        `cp /vagrant/backup_20150402_0021.zip /var/lib/jenkins/backup`
    - set up ssh key so that Jenkins can connect to Github
        - `ssh-keygen -t rsa -C "your_email@example.com"`  
        to create a new ssh key, using the provided email as a label. Accept defaults.
        - `cat ~/.ssh/id_rsa.pub`
        - copy the output of the above command into the clipboard
        - go to `https://github.com/your-github-user-name/your-github-repo/settings/keys`
        - add a new deploy key with the `Title` of your choosing and paste the contents  
        of ~/.ssh/id_rsa.pub from your clipboard as the `Key`
        - test the connection by running  
        `ssh -T git@github.com`  
            - when prompted, type `yes`
            - you are done when the output is  
            `Hi username/repository! You've successfully authenticated, but GitHub does not provide shell access.`
    - configure Nexus access for Maven
        - `cp /vagrant/settings-template.xml /var/lib/jenkins/.m2/settings.xml`
        - edit the copied settings.xml to replace the username and password with the credentials of the Nexus user permitted to deploy artifacts
    - restore jenkins to source-controlled baseline configuration
        - go to `http://192.168.50.7:8080/pluginManager/available` and install `Backup plugin` plugin
        - configure backup directory at `http://192.168.50.7:8080/backup/backupsettings` to be `/var/lib/jenkins/backup`
        - go to `http://192.168.50.7:8080/backup/launchrestore`, choose the restore file you copied earlier and select `Launch restore`
        - push `Launch restore` button

You should now have a provisioned VM running Jenkins.  It is configured so that your Virtual Machine is accessible at ````192.168.50.7````

To access the Nexus server, point your browser at http://192.168.50.4:8080
