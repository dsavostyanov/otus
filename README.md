# Первые шаги с Ansible
```
Vagrantfile:

MACHINES = {
  :nginx => {
    :box_name => "generic/ubuntu2204",
    :vm_name => "nginx",
    :net => [
      ["192.168.11.150", 2, "255.255.255.0", "mynet"],
    ]
  },
  :ansible => {
    :box_name => "generic/ubuntu2204",
    :vm_name => "ansible",
    :net => [
      ["192.168.11.151", 2, "255.255.255.0", "mynet"],
    ]
  }
}

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxconfig[:vm_name]

      box.vm.provider "virtualbox" do |v|
        v.memory = 768
        v.cpus = 1
      end

      boxconfig[:net].each do |ipconf|
        box.vm.network("private_network", ip: ipconf[0], adapter: ipconf[1], netmask: ipconf[2], virtualbox__intnet: ipconf[3])
      end

      if boxconfig.key?(:public)
        box.vm.network "public_network", boxconfig[:public]
      end

      box.vm.provision "shell", inline: <<-SHELL
        mkdir -p ~root/.ssh
        cp ~vagrant/.ssh/auth* ~root/.ssh
        sudo sed -i 's/\\#PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
        systemctl restart sshd
      SHELL
    end
  end
end


vagrant@ansible:~$ sudo -i

vagrant@ansible:~$ apt update

vagrant@ansible:~$ apt install software-properties-common

vagrant@ansible:~$ add-apt-repository --yes --update ppa:ansible/ansible

vagrant@ansible:~$ apt install ansible

vagrant@ansible:~/ansible-nginx$ vi staging/hosts
[web]
nginx ansible_host=192.168.11.150  ansible_port=22 ansible_user=vagrant ansible_private_key_file=~/.ssh/private_key


vagrant@ansible:~/ansible-nginx$ ansible nginx -i staging/hosts -m ping
[WARNING]: Platform linux on host nginx is using the discovered Python interpreter at /usr/bin/python3.10, but future installation of another Python interpreter could change the meaning of
that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
nginx | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.10"
    },
    "changed": false,
    "ping": "pong"
}

vagrant@ansible:~/ansible-nginx$ touch ansible.cfg

vagrant@ansible:~/ansible-nginx$ vi ansible.cfg
[defaults]
inventory = staging/hosts
remote_user = vagrant
host_key_checking = False
retry_files_enabled = False

vagrant@ansible:~/ansible-nginx$ vi staging/hosts
[web]
nginx ansible_host=192.168.11.150  ansible_port=22 ansible_private_key_file=~/.ssh/private_key

vagrant@ansible:~/ansible-nginx$ ansible nginx -m ping
[WARNING]: Platform linux on host nginx is using the discovered Python interpreter at /usr/bin/python3.10, but future installation of another Python interpreter could change the meaning of
that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
nginx | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.10"
    },
    "changed": false,
    "ping": "pong"
}

vagrant@ansible:~/ansible-nginx$ ansible nginx -m command -a "uname -r"
[WARNING]: Platform linux on host nginx is using the discovered Python interpreter at /usr/bin/python3.10, but future installation of another Python interpreter could change the meaning of
that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
nginx | CHANGED | rc=0 >>
5.15.0-91-generic

vagrant@ansible:~/ansible-nginx$ ansible nginx -m systemd -a name=firewalld
[WARNING]: Platform linux on host nginx is using the discovered Python interpreter at /usr/bin/python3.10, but future installation of another Python interpreter could change the meaning of
that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
nginx | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.10"
    },
    "changed": false,
    "name": "firewalld",
    "status": {
        "ActiveEnterTimestamp": "n/a",
        "ActiveEnterTimestampMonotonic": "0",
        "ActiveExitTimestamp": "n/a",
        "ActiveExitTimestampMonotonic": "0",
        "ActiveState": "inactive",
        "AllowIsolate": "no",
        "AssertResult": "no",
        "AssertTimestamp": "n/a",
        "AssertTimestampMonotonic": "0",
        "BlockIOAccounting": "no",
        "BlockIOWeight": "[not set]",
        "CPUAccounting": "yes",
        "CPUAffinityFromNUMA": "no",
        "CPUQuotaPerSecUSec": "infinity",
        "CPUQuotaPeriodUSec": "infinity",
        "CPUSchedulingPolicy": "0",
        "CPUSchedulingPriority": "0",
        "CPUSchedulingResetOnFork": "no",
        "CPUShares": "[not set]",
        "CPUUsageNSec": "[not set]",
        "CPUWeight": "[not set]",
        "CacheDirectoryMode": "0755",
...........

vagrant@ansible:~/ansible-nginx/playbooks$ touch nginx.yml

vagrant@ansible:~/ansible-nginx/playbooks$ vi nginx.yml

---
- name: Nginx | Install and configure NGINX
  hosts: nginx
  become: true

  tasks:
    - name: update cache
      apt:
        update_cache: yes

    - name: Nginx | Install Nginx
      apt:
        name: nginx
        state: latest
		
vagrant@ansible:~/ansible-nginx$ ansible-playbook -i staging/hosts playbooks/nginx.yml

PLAY [Nginx | Install and configure NGINX] ***************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************
[WARNING]: Platform linux on host nginx is using the discovered Python interpreter at /usr/bin/python3.10, but future installation of another Python interpreter could change the meaning of
that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
ok: [nginx]

TASK [update cache] **************************************************************************************************************************************************************************
changed: [nginx]

TASK [Nginx | Install Nginx] *****************************************************************************************************************************************************************
changed: [nginx]

PLAY RECAP ***********************************************************************************************************************************************************************************
nginx                      : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0



nginx.yml:

---
- name: Nginx | Install and configure NGINX
  hosts: nginx
  become: true

  vars:
    nginx_listen_port: 8080

  tasks:
    - name: update cache
      apt:
        update_cache: yes
      tags:
        - update apt

    - name: Nginx | Install Nginx
      apt:
        name: nginx
        state: latest
      notify:
        - restart nginx
      tags:
        - nginx-package

    - name: NGINX | Deploy nginx.conf from template
      template:
        src: templates/nginx.conf.j2
        dest: /etc/nginx/nginx.conf
      notify:
        - reload nginx
      tags:
        - nginx-configuration

  handlers:
    - name: restart nginx
      systemd:
        name: nginx
        state: restarted
        enabled: yes

    - name: reload nginx
      systemd:
        name: nginx
        state: reloaded
		


vagrant@ansible:~/ansible-nginx$ ansible-playbook -i staging/hosts playbooks/nginx.yml

PLAY [Nginx | Install and configure NGINX] ***************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************
[WARNING]: Platform linux on host nginx is using the discovered Python interpreter at /usr/bin/python3.10, but future installation of another Python interpreter could change the meaning of
that path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
ok: [nginx]

TASK [update cache] **************************************************************************************************************************************************************************
changed: [nginx]

TASK [Nginx | Install Nginx] *****************************************************************************************************************************************************************
ok: [nginx]

TASK [NGINX | Deploy nginx.conf from template] ***********************************************************************************************************************************************
changed: [nginx]

RUNNING HANDLER [reload nginx] ***************************************************************************************************************************************************************
changed: [nginx]

PLAY RECAP ***********************************************************************************************************************************************************************************
nginx                      : ok=5    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0


vagrant@ansible:~/ansible-nginx$ curl http://192.168.11.150:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>



```
