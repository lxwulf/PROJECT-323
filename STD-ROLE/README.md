# STD-ROLE

A "standard" role which set up every new machine with different tools that I use frequently like `tmux`, `wget` or `git` for example.
It also upgrades the machines and deactivate ipv6 support to remove some possible errors which can happens.

## Requirements

The only requirements are mitogen plugin and a linux os like fedora.

## Role Variables

The role variables are in `STD-ROLE/vars/main.yml`. The host variable are in my ssh-config file and in `STD-ROLE/group_vars/dev.yml` which is not included. You can setup your own file which must have the same name as the group defined in the `hosts` file.
The `target` variable in the playbook(s) are always the for the command line option `--extra-vars`.

### Example

./hosts

```bash
[dev]
ansible
```

./group_vars/dev.yml

```yml
#user std logins
ansible_user: yourusername_of_the_remote_machine
ansible_ssh_pass: your_password_of_the_remote_machine
ansible_become_password: your_password_of_the_remote_machine
ansible_python_interpreter: /usr/bin/python3
```

ssh config file mostly under ~/.ssh/config

```bash
host ansible
  HostName 192.168.1.121
  User remotehostusername
  Port remoteport
```

The variables for the `--extra-vars` are used in the following way:

```yml
ansible-playbook std-playbook.yml --extra-vars target=pro
```

At the end, `pro` is the variable you pass to the playbook. So you can choose which hosts you want to use to play the role.
The `pro` variable is from the host file, so there are also `dev` and `tes` to use.

## Dependencies

There are no dependencies. All special modules should be provided by ansible itself.

## Example Playbook

So far no special variable are declared within command line. But you can expand or modify
the package list as you want under `STD-ROLE/vars/main.yml`.

## Author Information

[My Github Profile](https://github.com/lxwulf)
