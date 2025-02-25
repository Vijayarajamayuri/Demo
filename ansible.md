<details>
<summary><b> 1. Installation of Ansible </b></summary>
Ansible’s only real dependency is Python. Once Python is installed, the simplest way to get Ansible running is to use pip, a simple package manager for Python.

### Verify the version of the Python

`python3 --version`

> If you don't have python3 installed on your local you could install with [link to installation](https://docs.python.org/3/using/windows.html)


### Installation of the Ansible 

`pip3 install ansible`


### Verify the Version

`ansible --version`

Example output:
```
ansible [core 2.15.12]
  ...
  python version = 3.9.19 (main, Dec 18 2024, 20:53:49) [GCC 11.2.0] (/opt/ansible/bin/python)
  jinja version = 3.1.4
  libyaml = True
```


</details>


<details>
<summary><b> 2. Basics of Ansible  </b></summary>

### Running `Hello World` with Ansible on your local machine

This playbook is a simple example that demonstrates running a command locally and displaying its output. 

Create a directory called local and inside it create a file named playbook.yml with the following content:

```
- hosts: localhost
  name: Hello World!
  connection: local
  tasks:
    - command: echo Hello World!
      register: result
    - debug:
        var: result.stdout
```


`hosts: localhost` : The playbook targets the local machine, meaning the tasks will run on the machine where you execute the playbook.

`connection: local` : This tells Ansible to use a local connection instead of SSH. It's useful for testing or when you want to run tasks on your own computer.

`tasks:` The playbook defines two tasks 

  - `Command` echo Hello World! : Executes the shell command echo Hello World! to print the message.
    - `register: result`: Captures the output of the command in a variable called result.
  - `Debug` : Uses the debug module to print variables.
    - `result.stdout` : Displays the standard output  of the previous command, effectively showing "Hello World!" on the screen.




### Working with Remote VM
Lets run first adhoc command with ansible using following, please update your `IP_ADDRESS` and don't forget to add `,` after IP_ADDRESS 


```
ansible all -i "IP_ADDRESS," -m ping -u azureuser
```

Ansible uses an inventory file to communicate with  servers. Like a hosts file (at /etc/hosts) that matches IP addresses to domain names, an Ansible inventory file matches servers (IP addresses or domain names) to groups. Inventory files can do a lot more, but for now, we’ll just create a simple file with one
server. Create a file named inventory.ini in a new directory called remote:


```
$ mkdir remote
$ cd remote
$ touch inventory.ini
```
Edit this hosts file with vim. Put the Public IP of the virtual machine into the file:

``` 
[vm]
PUBLIC_IP
```
### Running your first Ad-Hoc Ansible command
Now that you’ve installed Ansible and created an inventory file, it’s time to run a command to test connectivity and gather facts Enter the following in the terminal:



```
ansible -i inventory.ini vm -m ping -u azureuser -vvvv
```

Expected output:
```
172.174.132.102 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```
`-i inventory.ini` : Specifies the inventory file that lists your hosts.
`vm`: Targets the host group named “vm” defined in the inventory.
`-m` : Specifies module name, tries to connect to host, verify a usable python and return pong on success 
`-vvvv` : Increases verbosity for detailed debugging output
When executed, Ansible reads inventory.ini for hosts under the “vm” group and connects to each host to gather and display system information in JSON format.




Now lets run another usefull command to check memory usage on the server 
```
$ ansible -i inventory.ini vm -a "free -h" -u azureuser
```


Following command runs an Ansible ad-hoc command that gathers system information (facts) from hosts defined in the inventory file.

```
$ ansible -i inventory.ini vm -m setup  -u azureuser
```


`-m setup`: Invokes the setup module, which collects detailed system facts (e.g., OS, network, hardware).




### First Ansible Playbook
Let’s create the Ansible playbook.yml file now. Create an empty text file in the same folder as your inventory.ini, and put in the following contents:



```yaml
---
- hosts: vm 
  become: yes

  tasks:
  - name: Ensure chrony for time sync is installed.
    apt:
      name: chrony
      state: present

  - name: Ensure chrony is running.
    service:
      name: chrony
      state: started
      enabled: yes
```

Lets run following command 

`ansible-playbook playbook.yml -i inventory.ini -u azureuser`


### Ansible Configuration file 

Now that you've run a few playbooks, it's important to understand how to customize Ansible's behavior using the ansible.cfg file. Lets create it using `touch ansible.cfg` or `vim ansible.cfg` 
```yaml
[defaults]
inventory = ./inventory.ini
remote_user = azureuser
```
and run one more time same command without specifying `-u` 

`ansible-playbook playbook.yml -i inventory.ini`
</details>





<details>
<summary><b> 3. Working with multiple servers  </b></summary>

Our environment is growing, so let's update our inventory file as follows: 


```
[web]
IP_ADDRESS_WEB_SERVER

[db]
IP_ADDRESS_DB


[multi:children]
web
db

[multi:vars]
ansible_user = azureuser
```

### Play 1 (Update packages):
Lets update every server we have on our inventory files

`hosts: all`: Targets every host in your inventory.
`become`: yes: Runs tasks with elevated privileges.
apt module: Updates the apt cache and performs a distribution upgrade.

lets create file called update.yml
```
---
- name: Update packages on all Ubuntu hosts
  hosts: all
  become: yes
  tasks:
    - name: Update apt cache and upgrade packages
      apt:
        update_cache: yes
        upgrade: dist
```

#### Play 2 (Web server):

lets create file called web.yml and paste following 
```
---
- name: Install Apache on web servers and configure firewall
  hosts: web
  become: yes
  tasks:
    - name: Install Apache2
      apt:
        name: apache2
        state: latest

    - name: Ensure Apache2 service is started and enabled
      systemd:
        name: apache2
        state: started
        enabled: yes

    - name: Open TCP ports 80 and 443 using ufw
      ufw:
        rule: allow
        port: "{{ item }}"
        proto: tcp
      loop:
        - 80
        - 443
```
`hosts: web` : Applies only to hosts in the "web" group.
`Install Apache2`: Uses the apt module to install Apache.
`systemd module`: Ensures the Apache service is running and enabled at boot.
`ufw module`: Opens ports 80 and 443 for HTTP and HTTPS traffic.



#### Play 3 (DB server):
lets create file called db.yml and paste following 


```
---
- name: Install MySQL on db servers and start service
  hosts: db
  become: yes
  tasks:
    - name: Install MySQL Server
      apt:
        name: mysql-server
        state: latest

    - name: Ensure MySQL service is started and enabled
      systemd:
        name: mysql
        state: started
        enabled: yes
```
`hosts: db `: Applies only to hosts in the "db" group.
`Install MySQL Server` : Installs the MySQL server package via apt.
`systemd module`: Starts the MySQL service and enables it to run at boot.

This playbook assumes that your inventory file is set up with groups named web and db for the appropriate hosts. Adjust package names or service names if your environment differs.



and finally create file called site.yml and paste following; 
```
---
- import_playbook: update.yml
- import_playbook: web.yml
- import_playbook: db.yml
```
Modular Playbooks: Each file (update.yml, web.yml, db.yml) contains a specific set of tasks targeting different groups of hosts.

Master Playbook (site.yml): The site.yml file uses the import_playbook directive to include each of the separate playbooks. This means when you run:

`ansible-playbook -i inventory.ini site.yml`


Ansible will sequentially execute the tasks defined in `update.yml`, `webserver.yml`, and `db.yml`.



Finally lets add ansible.cfg file to current directory as follows: 

```yaml
[defaults]
inventory = ./inventory.ini
remote_user = azureuser
```

Run following command 
`ansible-playbook  site.yml`

</details>
