# Security (`security`)

**Part of the BAS Ansible Role Collection (BARC)**

Implements a number of steps to improve the security of a VM by configuration services and using security packages and firewalls 

## Overview

* Optionally, installs and configures the Unattended Upgrades package to automatically install security upgrades on a daily basis, this is enabled by default
* Optionally, configures the SSH daemon to restrict by which methods users and root can login remotely (i.e. public key only), this is enabled by default
* Optionally, installs and configures the fail2ban package to automatically block, using the firewall, access to SSH after too many incorrect logins in a set period, this is enabled by default
* Optionally, installs and configures the UFW package to more easily configure the system firewall for common scenarios, this is enabled by default

## Availability

This role is designed for internal use but if useful can be shared publicly.

## Usage

### Requirements

#### BAS Ansible Role Collection (BARC)

* `core`

### Variables

* `security_enable_feature_unattended_upgrades`
    * If "true" the unattended upgrades package will be configured to perform package updates (i.e. `apt-get upgrade`) automatically on a defined schedule
    * This is a binary variable and **MUST** be set to either "true" or "false" (without quotes).
    * Default: "true"
* `security_unattended_upgrades_unattended_upgrade_periodic_interval`
    * Determines how often, in days, selected upgrades should be applied
    * This value **MUST** be a string consisting of an integer value without a unit
    * Default: "1"
* `security_enable_feature_sshd_config`
    * If "true" the SSH daemon will be configured to increase it's security, mainly focused on how users can login and from where
    * This is a binary variable and **MUST** be set to either "true" or "false" (without quotes).
    * Default: "true"
* `security_sshd_config_permit_root_login`
    * Determines if and how the root user can login using SSH
    * This variable **MUST** be a valid value, as determined by SSH, valid options are "yes", "no" and "without-password".
    * See [here](http://askubuntu.com/a/449372) for guidance on how to set this value.
    * By default, this variable is set to "without-password" so that bootstrap roles (which will login as root) can still run, therefore you **SHOULD NOT** set this to "no"
    * Default: "without-password"
* `security_sshd_config_password_authentication`
    * Determines if standard passwords can be used to login using SSH
    * This is a binary variable and **MUST** be set to either "yes" or "no" and **MUST** be quoted.
    * Default: "no"
* `security_enable_feature_fail2ban`
    * If "true" the fail2ban package will be installed and configured, this uses the system firewall to block users who fail to authenticate to a service too many incorrect logins in a set period
    * This is a binary variable and **MUST** be set to either "true" or "false" (without quotes).
    * Default: "true"
* `security_fail2ban_jail_local_defaults_bantime`
    * The length of time a banned user is banned for
    * This value **MUST** be an integer value without a unit
    * Default: "1800" (30 minutes)
* `security_fail2ban_jail_local_defaults_findtime`
    * The length of a login "period" over which incorrect logins will be 'grouped'
    * This value **MUST** be an integer value without a unit
    * Default: "600" (10 minutes)
* `security_fail2ban_jail_local_defaults_maxretry`
    * The number of login attempts allowed in a login "period" after which a user will be banned
    * This value **MUST** be a string consisting of an integer value without a unit
    * Default: "3"
* `security_fail2ban_jail_local_ssh_enabled`
    * If "true", a user making too many incorrect SSH logins in the "login" period will be banned
    * This is a binary variable and **MUST** be set to either "true" or "false" and **MUST** be quoted.
    * Default: "true"
* `security_enable_feature_ufw`
    * If "true" the Uncomplicated Firewall package will be installed and configured with default policies to allow all output and block all inbound traffic
    * This is a binary variable and **MUST** be set to either "true" or "false" (without quotes).
    * Default: "true"
* `security_ufw_apply_default_policy_deny_incoming`
    * If "true", the default policy to deny all incoming connections will be applied
    * This is a conventional default and **SHOULD NOT** be changed.
    * Although this policy should be set by default this policy is explicitly enabled to ensure a consistent firewall 'profile' is used across all VMs this role is applied to.
    * This is a binary variable and **MUST** be set to either "true" or "false" (without quotes).
    * Default: "true"
* `security_ufw_apply_default_policy_allow_outgoing`
    * If "true", the default policy to allow all outgoing connections will be applied
    * This is a conventional default and **SHOULD NOT** be changed.
    * Although this policy should be set by default this policy is explicitly enabled to ensure a consistent firewall 'profile' is used across all VMs this role is applied to.
    * This is a binary variable and **MUST** be set to either "true" or "false" (without quotes).
    * Default: "true"
* `security_ufw_allow_incoming_ssh`
    * If "true", a rule to allow incoming connections on port 22 (SSH) will be added
    * This rule **MUST** be enabled to allow Ansible to connect to a VM in the future, if "false" all remote sessions, including those initiated manually be refused.
    * This is a conventional default and **SHOULD NOT** be changed.
    * This is a binary variable and **MUST** be set to either "true" or "false" (without quotes).
    * Default: "true"
* `security_ufw_allowed_incoming_tcp_ports`
    * If "true", rules to allow a range of incoming tcp ports will be added
    * This variable will allow conventional ports for HTTP(s) - specifically ports `80,443,8080,8443` - this is deprecated behaviour.
    * In future these defaults will be removed as individual applications (in their own roles/tasks) should be responsible for adding any firewall rules they require themselves.
    * This variables uses an array of integer values, each value **MUST** be a valid UNIX port.
    * Default: (Array)
        * 80
        * 443
        * 8080
        * 8443

## Contributing

This project welcomes contributions, see `CONTRIBUTING` for our general policy.

## Developing

### Committing changes

The [Git flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow/) workflow is used to manage development of this package.

Discrete changes should be made within *feature* branches, created from and merged back into *develop* (where small one-line changes may be made directly).

When ready to release a set of features/changes create a *release* branch from *develop*, update documentation as required and merge into *master* with a tagged, [semantic version](http://semver.org/) (e.g. `v1.2.3`).

After releases the *master* branch should be merged with *develop* to restart the process. High impact bugs can be addressed in *hotfix* branches, created from and merged into *master* directly (and then into *develop*).

### Issue tracking

Issues, bugs, improvements, questions, suggestions and other tasks related to this package are managed through the BAS Web & Applications Team Jira project ([BASWEB](https://jira.ceh.ac.uk/browse/BASWEB)).

## License

Copyright 2015 NERC BAS. Licensed under the MIT license, see `LICENSE` for details.
