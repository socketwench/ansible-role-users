# Ansible Role: Users and Groups

Creates users and groups for Debian/Ubuntu Linux.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`).

To create a user with all defaults, and who's primary group is the same as their username:

    server_users:
      - name: "ash"
        password: "imnotarobot"

### Groups

There is no separate groups variable. They are instead part of the `server_users` variable.

To specify the primary group name and alternate groups:

    server_users:
      - name: "ash"
        password: "imnotarobot"
        group: "primarygroupname"
        groups:
          - "anothergroup"
          - "more-groups-here"

### SSH Keys

By default, all users created by this role are generated a new SSH key:

    server_users_ssh_key_generate: yes
    server_users_ssh_key_bits: "4096"

You can override this per user too:

    server_users:
      - name: "ash"
        password: "imnotarobot"
        ssh_key_generate: yes
        ssh_key_bits: "4096"

### SSH Authorized keys

Sometimes you want to also set the authorized keys so people can log in using
public keys instead of passwords. To set that for all users:

server_users_auth_keys: "{{ contents_of_ssh_authorized_keys }}"

Where the contents of the variable is the same as the `~/.ssh/authorized_keys`
files.

You can also override this per user:

    server_users:
      - name: "ash"
        password: "imnotarobot"
        auth_keys: "{{ contents_of_ssh_authorized_keys }}"

### Default shell

The default shell is bash. To can override this for all users:

    server_users_shell: "/bin/bash"

You can also specify a shell for a particular user. This overrides the above default:

    server_users:
      - name: "ash"
        password: "imnotarobot"
        shell: "/bin/zsh"

By default, the role does not log task actions for security reasons. To enable logging:

    server_users_no_log: true

### Ansible configurations

This role also distributes an `.ansible.cfg` file to each user's home directory.
This allows you to override the default role_path and no_cows settings:

    server_users_ansible_role_path:  "~/.ansible/roles"
    server_users_ansible_nocows: 1

Again, you can do this per user:

    server_users:
      - name: "ash"
        password: "imnotarobot"
        ansible_role_path: "/etc/ansible/roles"
        ansible_nocows: 0

## Dependencies

None.

## Example Playbook

    server_users:
      - name: "ash"
        password: "imnotarobot"
        group: "wy"
        groups:
          - "scidiv"
      - name: "kane"
        password: "ihateeggs"
        shell: "/bin/ksh"
        ssh_key_generate: no

## License

GPL 3.0.

## Author Information

This role was created in 2017 by [socketwench](https://deninet.com/).
