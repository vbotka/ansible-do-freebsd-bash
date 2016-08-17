do-freebsd-bash
===============

This role installs /usr/local/bin/bash and changes the login shell
for user freebsd to bash (pw usermod freebsd -s /usr/local/bin/bash)

Default login shell in FreeBSD is /bin/tcsh. This doesn't work properly with
ansible as discussed in issues:

- [fatal error caused by shell type]
(https://github.com/ansible/ansible/issues/13459)

- [ansible_shell_type and make_become_cmd are at odds]
(https://github.com/ansible/ansible/issues/13179)

This role was tested with fresh droplet 10.3-RELEASE FreeBSD at
https://cloud.digitalocean.com.


Requirements
------------

Before applying this role it's necessary to change the login shell

```
ansible do-bsd-test -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'

```


Variables
---------

```
do_freebsd_bash_user	# user whos login shell will be changed
```


Workflow
----------------

1) Install the role from Ansible Galaxy https://galaxy.ansible.com/

```
ansible-galaxy install vbotka.ansible-do-freebsd-bash
- downloading role 'ansible-do-freebsd-bash', owned by vbotka
- downloading role from https://github.com/vbotka/ansible-do-freebsd-bash/archive/master.tar.gz
- extracting vbotka.ansible-do-freebsd-bash to /home/vlado/.ansible/roles/vbotka.ansible-do-freebsd-bash
- vbotka.ansible-do-freebsd-bash was installed successfully
```

2) Edit inventory

```
hosts

[do-bsd-test]
139.59.214.27

[do-bsd-test:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_python_interpreter=/usr/local/bin/python2
ansible_perl_interpreter=/usr/local/bin/perl
```

3) Change shell (if necessary)

```
ansible do-bsd-test -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
139.59.214.27 | SUCCESS | rc=0 >>
```

4) Edit playbook

```
cat ~/.ansible/playbooks/do-freebsd-bash.yml
---
- hosts: do-bsd-test
roles:
      - role: vbotka.ansible-do-freebsd-bash
```

5) Run playbook

```
ansible-playbook ~/.ansible/playbooks/do-freebsd-bash.yml

PLAY ***************************************************************************
TASK [setup] *******************************************************************
ok: [139.59.214.27]
TASK [vbotka.ansible-do-freebsd-bash : include] ********************************
included: /home/vlado/.ansible/roles/vbotka.ansible-do-freebsd-bash/tasks/packages.yml for 139.59.214.27
TASK [vbotka.ansible-do-freebsd-bash : Install base packages] ******************
changed: [139.59.214.27]
TASK [vbotka.ansible-do-freebsd-bash : include] ********************************
included: /export/home/vlado.config/.ansible/roles/vbotka.ansible-do-freebsd-bash/tasks/shell.yml for 139.59.214.27
TASK [vbotka.ansible-do-freebsd-bash : change default shell to bash for user freebsd] ***
changed: [139.59.214.27]
PLAY RECAP *********************************************************************
139.59.214.27              : ok=5    changed=2    unreachable=0    failed=0
```

License
-------

BSD


Author Information
------------------

Vladimir Botka https://botka.link
