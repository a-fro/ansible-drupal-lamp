# Ansible Role: Security (Basics)

[![Build Status](https://travis-ci.org/geerlingguy/ansible-role-security.svg?branch=master)](https://travis-ci.org/geerlingguy/ansible-role-security)

**First, a major, MAJOR caveat**: the security of your servers is YOUR responsibility. If you think simply including this role and adding a firewall makes a server secure, then you're mistaken. Read up on Linux, network, and application security, and know that no matter how much you know, you can always make every part of your stack more secure.

That being said, this role performs some basic security configuration on RedHat and Debian-based linux systems. It attempts to:

  - Install software to monitor bad SSH access (fail2ban)
  - Configure SSH to be more secure (disabling root login, requiring key-based authentication, and allowing a custom SSH port to be set)
  - Set up automatic updates (if configured to do so)

There are a few other things you may or may not want to do (which are not included in this role) to make sure your servers are more secure, like:

  - Use logwatch or a centralized logging server to analyze and monitor log files
  - Securely configure user accounts and SSH keys (this role assumes you're not using password authentication or logging in as root)
  - Have a well-configured firewall (check out the `geerlingguy.firewall` role on Ansible Galaxy for a flexible example)

Again: Your servers' security is *your* responsibility.

## Requirements

For obvious reasons, `sudo` must be installed if you want to manage the sudoers file with this role.

On RedHat/CentOS systems, make sure you have the EPEL repository installed (you can include the `geerlingguy.repo-epel` role to get it installed).

No special requirements for Debian/Ubuntu systems.

## Role Variables

Available variables are listed below, along with default values (see `vars/main.yml`):

    security_ssh_port: 22

The port through which you'd like SSH to be accessible. The default is port 22, but if you're operating a server on the open internet, and have no firewall blocking access to port 22, you'll quickly find that thousands of login attempts per day are not uncommon. You can change the port to a nonstandard port (e.g. 2849) if you want to avoid these thousands of automated penetration attempts.

    security_sudoers_passwordless: []
    security_sudoers_passworded: []

A list of users who should be added to the sudoers file so they can run any command as root (via `sudo`) either without a password or requiring a password for each command, respectively.

    security_autoupdate_enabled: true

Whether to install/enable `yum-cron` (RedHat-based systems) or `unattended-upgrades` (Debian-based systems). System restarts will not happen automatically in any case, and automatic upgrades are no excuse for sloppy patch and package management, but automatic updates can be helpful as yet another security measure.

## Dependencies

None.

## Example Playbook

    - hosts: servers
      vars_files:
        - vars/main.yml
      roles:
        - { role: geerlingguy.security }

*Inside `vars/main.yml`*:

    security_sudoers_passworded:
      - johndoe
      - deployacct

## License

MIT / BSD

## Author Information

This role was created in 2014 by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](http://ansiblefordevops.com/).
