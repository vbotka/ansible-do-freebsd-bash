do-freebsd-bash
===============

This role installs /usr/local/bin/bash and changes the default shell
for user freebsd to bash (pw usermod freebsd -s /usr/local/bin/bash)

Default shell in FreeBSD is /bin/tcsh. This doesn't work properly with
ansible as discussed in bugs:

fatal error caused by shell type
https://github.com/ansible/ansible/issues/13459

ansible_shell_type and make_become_cmd are at odds
https://github.com/ansible/ansible/issues/13179

This role was tested with fresh droplet 10.3-RELEASE FreeBSD at
https://cloud.digitalocean.com.


Requirements
------------

Before applying this role it's necessary to change the default shell
for user freebsd to /bin/sh.

```
pw usermod freebsd -s /bin/sh
```

Examples
----------------

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

```
cat ~/.ansible/playbooks/do-freebsd-bash.yml
---
- hosts: do-bsd-test
roles:
      - role: do-freebsd-bash
```

```
ansible-playbook ~/.ansible/playbooks/do-freebsd-bash.yml
PLAY ***************************************************************************
TASK [setup] *******************************************************************
ok: [139.59.214.27]
TASK [do-freebsd-bash : include] ***********************************************
included: /home/vlado/.ansible/roles/do-freebsd-bash/tasks/packages.yml for 139.59.214.27
TASK [do-freebsd-bash : Install base packages] *********************************
changed: [139.59.214.27]
TASK [do-freebsd-bash : include] ***********************************************
included: /home/vlado/.ansible/roles/do-freebsd-bash/tasks/shell.yml for 139.59.214.27
TASK [do-freebsd-bash : change default shell to bash for user freebsd] *********
changed: [139.59.214.27]
PLAY RECAP *********************************************************************
139.59.214.27              : ok=5    changed=2    unreachable=0    failed=0
```

License
-------

BSD


Author Information
------------------

https://botka.link
