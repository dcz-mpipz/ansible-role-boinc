# Boinc

[![Ansible Galaxy][galaxy_image]][galaxy_link]
[![Build Status][travis_image]][travis_link]
[![Latest tag][tag_image]][tag_url]
[![Gitter chat][gitter_image]][gitter_url]

A role for managing BOINC clients. The role installs the BOINC client and lets
you manage it. There are two options for managing your projects:

- Using an account manager like BAM.
    If you would like to use an account manager you need do set
    `boinc_acct_mgr` to 'yes' and specify the URL, username and password for
    the manager.
- Using this role.
    If you would like to use this role for management you need to specify a
    list of projects and there information.
    This feature is rather limited at the moment. Contributions are welcome!
    (Please make a feature request on GitHub if you realy need more control.)

## Requirements

- Hosts should be bootstrapped for ansible usage (have python,...)
- Root privileges, eg `become: yes`

## Role Variables

| Variable | Description | Default value |
|----------|-------------|---------------|
| `boinc_state` | State of the BOINC client (enabled/disabled) | 'enabled' |
| `boinc_acct_mgr` | Use an account manager? | 'no' |
| `boinc_acct_mgr_url` | Account manager URL | / |
| `boinc_acct_mgr_pass` | Account manager password | / |
| `boinc_acct_mgr_user` | Account manager user name | / |
| `boinc_project_state` | Default project state (enabled/disabled) | 'enabled' |
| `boinc_project_list` | List of projects **(see details!)** | `[]` |

#### `boinc_project_list` details

List of projects with there URL and authentication key. Automatic creation of
accounts or username/password authentication is not yet supported.

Each project in the list can have following attributes:

| Variable | Description | Required | Default value |
|----------|-------------|----------|---------------|
| `url` | URL of the project | yes | / |
| `key` | Authentication key for the project | if enabled | / |
| `state` | State of the project (enabled/disabled) | no | `boinc_project_state` |
| `action` | Action to perform for this project | no | / |

#### Attention:
All boolean values can be used with either `'yes'`/`'no'` or `true`/`false`.
This allows you to alter their value from the command line (`-e "bool=yes"`)
without problems.

Registering a client to an account manager will not be marked as a change.

Project actions are performed every time you run the playbook. You should specify
these from the command line using the `-e` option to avoid problems.

## Dependencies

- [GROG.package][grog.package]

## Example Playbook

#### Using BAM account manager:

```yaml
---
- hosts: boinc
  vars:
    boinc_acct_mgr: 'yes'
    boinc_acct_mgr_url: 'http://bam.boincstats.com'
    boinc_acct_mgr_user: 'name'
    boinc_acct_mgr_pass: 'secret'
  roles:
  - { role: GROG.boinc, become: yes }
```

#### Using a project list:

```yaml
---
- hosts: boinc
  vars:
    boinc_project_list:
      - url: 'http://einstein.phys.uwm.edu/'
        key: 'alskfjalskjfalkfjalfjaljflkfjaljfalskjf'
      - url: 'http://atlasathome.cern.ch/'
        key: 'alskfjalskjfalkfjalfjaljflkfjaljfalskjf'
  roles:
  - { role: GROG.boinc, become: yes }
```

## Contributing
All assistance, changes or ideas [welcome][issues]!

## Author
By [G. Roggemans][groggemans]

## License
MIT

[galaxy_image]:         https://img.shields.io/badge/galaxy-GROG.boinc-660198.svg?style=flat
[galaxy_link]:          https://galaxy.ansible.com/GROG/boinc
[travis_image]:         https://travis-ci.org/GROG/ansible-role-boinc.svg?branch=master
[travis_link]:          https://travis-ci.org/GROG/ansible-role-boinc
[tag_image]:            https://img.shields.io/github/tag/GROG/ansible-role-boinc.svg
[tag_url]:              https://github.com/GROG/ansible-role-boinc/tags
[gitter_image]:         https://badges.gitter.im/GROG/chat.svg
[gitter_url]:           https://gitter.im/GROG/chat

[grog.package]:         https://galaxy.ansible.com/GROG/package

[issues]:               https://github.com/GROG/ansible-role-boinc/issues
[groggemans]:           https://github.com/groggemans
