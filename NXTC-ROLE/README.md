# NXTC-ROLE

This is the role for automated installation of Nextcloud. The role set up Nextcloud with `PHP 7.4`, `PostgresSQL 13.4`, `Redis`,
`fail2ban` and `firewall-cmd`. At the end, you should have a working, secured cloud server with the logins you defined in the variables.

## Requirements

The only requirements are the mitogen plugin and a Linux OS like fedora.

## Role Variables

You find the settings in the role itself as variable. It should be declared what the task to do and which variable is needed.
The definition of the variables are in `NXTC-ROLE/vars/main.yml` which you need to create by your own.
There is a template file you can fill out or to create your own.

When you make your own, remember that you should define the variables with the same
name in the task, as example `NXTC-ROLE/tasks/01_modify_hostname.yml`, as also in the `NXTC-ROLE/vars/main.yml`. When you're using the
template file `NXTC-ROLE/vars/template.yml.bak`, after you fill it up you need to change it to the `.yml`, otherwise Ansible doesn't
care about the file.

### Example

Example of the use of variables.

#### NXTC-ROLE/tasks/01_modify_hostname.yml

```yml
---

- name: adding domain name in hosts (ipv4)
  ansible.builtin.lineinfile:
    path: /etc/hosts
    regexp: '^127\.0\.0\.1'
    line: 127.0.0.1 {{ fqdn_name }}
    owner: root
    group: root
    mode: '0644'
  become: yes

- name: adding domain name in hosts (ipv6)
  ansible.builtin.lineinfile:
    path: /etc/hosts
    regexp: '^::1'
    line: ::1 {{ fqdn_name }}
    owner: root
    group: root
    mode: '0644'
  become: yes

- name: adding external ip to domain in hosts (ext.ip)
  ansible.builtin.lineinfile:
    path: /etc/hosts
    insertafter: '^::1'
    line: "{{ realip }} {{ fqdn_name }}"
    owner: root
    group: root
    mode: '0644'
  become: yes

- name: adding domain name to hostname file
  ansible.builtin.lineinfile:
    path: /etc/hostname
    regexp: '.'
    line: "{{ fqdn_name }}"
    owner: root
    group: root
    mode: '0644'
  become: yes
```

#### NXTC-ROLE/vars/main.yml

```yml
---

fqdn_name: my-cloudserver.com
realip: 169.254.20.77
```

As said before you need to change this to your values and the filename from `NXTC-ROLE/vars/template.yml.bak` to `NXTC-ROLE/vars/main.yml`.

## Dependencies

There are no dependencies. All special modules should be provided by ansible itself.

## Example Playbook

So far there no special variables declared in this role. All variables are in `NXTC-ROLE/vars/main.yml`.

This is the playbook itself of this particular role

### nxtc-playbook.yml

```yml
---

- name: SETUP NXTC-ROLE
  hosts: "{{ target }}"
  strategy: mitogen_linear
  roles:
  - NXTC-ROLE
```

## Author Information

### [Github Profile](https://github.com/lxwulf)
