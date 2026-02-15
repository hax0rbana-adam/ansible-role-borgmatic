A simple role to install borgmatic and configure it to backup to a remote
server regularly.

If asking the role to get a SSH user certificate, you will need step (from
smallstep) installed and configured.

By default, it will use your PKI to set everything up:

1. Install borgmatic
2. Obtain SSH user certificates
3. Install a systemd service & timer to do backups

If you don't have your own PKI, that can be disabled and this role can use a
traditional keypair. The workflow for that looks like this:

1. Install borgmatic
2. Generate SSH keypair (if necessary)
3. Copy the backup server's SSH host key to known_hosts
4. Copy the public key to the backup server's authorized_keys
5. Install a systemd service & timer to do backups

# Examples
## Playbook
Here's an example of a playbook to install borgmatic on the local machine.
It does not require you have SSH running. In this example, the playbook
expects you to already have PKI set up, your backup server configured
to accept SSH user certificates obtained from said PKI, and the server being
backed up to trust SSH host keys which are signed by your PKI.

```yaml
- hosts: localhost
  connection: local
  become: true
  roles:
    - role: hax0rbana_adam.borgmatic
      borgmatic_borg_password: "In real life, put your password in an Ansible vault, not your playbook"
```

Here's an example of doing the same on remote hosts (whatever it in your
inventory). This one doesn't require any PKI, but instead will spin up
traditional SSH key pairs. It will also create the user account on the backup
server.

```yaml
- hosts: all
  become: true
  remote_user: root
  roles:
    - role: hax0rbana_adam.borgmatic
      borgmatic_borg_password: "In real life, put your password in an Ansible vault, not your playbook"
      borgmatic_backup_server: merlin@backup.example.com
      borgmatic_obtain_ssh_user_cert: false
      borgmatic_create_remote_user: true
      borgmatic_generate_keypair: true
      borgmatic_grab_known_hosts: true
      borgmatic_push_public_key: true
```

# Official repo location
All activity takes place on the official GitLab instance:
[https://gitlab.hax0rbana.org/public-repos/ansible/ansible-role-borgmatic](https://gitlab.hax0rbana.org/public-repos/ansible/ansible-role-borgmatic)

Any other hosting providers, such as GitHub.com and GitLab.com, are just mirrors
and we do not monitor the issue trackers over there.

# Support
## Matrix channel
You can also join our Matrix channel: #ansible:hax0rbana.org

This is a good place to ask questions or make requests without having to sign
up for another account.

# Contributing
See [contributor guidelines](CONTRIBUTING.md).

# License
This project is licensed under MIT License. See [LICENSE](LICENSE) for more details.
