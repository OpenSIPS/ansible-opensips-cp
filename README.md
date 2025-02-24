# OpenSIPS CP
This role installs the [OpenSIPS CP](https://controlpanel.opensips.org) tool.

Galaxy
----
Install your role using [Ansible
Galaxy](https://galaxy.ansible.com/razvancrainea/opensips_cp):

```
ansible-galaxy install razvancrainea.opensips_cp
```

Role Variables
----
The following variables can be set to tune the role's install behavior:
| Variable | Description | Default |
| -------- | ----------- | ------- |
| `opensips_cp_version` | the (git) version to install | `master` |
| `opensips_cp_dir` | the directory where the source files should be installed | `/var/www/html/opensips-cp` |
| `opensips_cp_db_engine` | Database engine to be used (currently only `mysql` and `postgres` is supported) | `mysql` |
| `opensips_cp_db_host` | Database host | `localhost` |
| `opensips_cp_db_port` | Database port | not used |
| `opensips_cp_db_user` | Database user | `opensips` |
| `opensips_cp_db_pass` | Database password | `opensipsrw` |
| `opensips_cp_db` | Database name | `opensips` |
| `opensips_cp_pass` | Password for the default `admin` user | `opensips` |
| `opensips_cp_stats` | Controls whether stats should be gathered | `true` |
| `opensips_cp_force_deploy` | Forces a deployment with the latest code | `false` |
| `opensips_cp_system_name` | Name of the OpenSIPS CP system | `SIP Servers` |
| `opensips_cp_system_description` | Description of the OpenSIPS CP system | `OpenSIPS SIP server cluster` |
| `opensips_mi_id` | OpenSIPS JSON-RPC listener's id | `1` |
| `opensips_mi_host` | OpenSIPS JSON-RPC listener's host | `127.0.0.1` |
| `opensips_mi_port` | OpenSIPS JSON-RPC listener's port | `8888` |
| `opensips_mi_root` | OpenSIPS JSON-RPC listener's root | `mi` |
| `opensips_mi_name` | OpenSIPS JSON-RPC listener's name | `SIP Server` |
| `opensips_mi_desc` | OpenSIPS JSON-RPC listener's description | `Default box` |
| `opensips_monit_host` | OpenSIPS Monit host | Undefined, not provided |
| `opensips_monit_port` | OpenSIPS Monit port | `2812`, useful only when `opensips_monit_host` is defined |
| `opensips_mi_boxes` | An array containing the `host`, `port`, `root` and `desc` nodes for each MI listener used | `An array with one box containing the `opensips_mi_*` variables |
| `opensips_cp_monit_host` | Host used by the `monit` tool | Value of `opensips_mi_host` |
| `opensips_cp_monit_port` | Port used by the `monit` tool | `2812` |
| `opensips_cp_modules` | Please view [Modules Configuration](#modules-configuration) | [`default_opensips_cp_modules`](vars/main.yml) |

Modules Configuration
----
The `opensips_cp_modules` variable can be used to tune the modules that are
being configured in OpenSIPS CP. It should be defined as a dictionary where
each element contains the specifications of a group (`users:`, `system:`).
Each group is another dictionary that contains one (or more) of the following
keys:
 * `name`: the name of the group, as it should appear in the web interface
 * `icon`: path to the icon that shall be used for the group
 * `modules`: a dictionary containing elements for each module to be used.
		 Example: `{ rtpproxy: RTPProxy, monit: Monit}`)
If one of the key is missing, it is taken from the
[`default_opensips_cp_modules`](vars/main.yml) variable, if it is defined, or
`''` otherwise. Each module can also be built as a dictionary, containg one of
the following keys:
   * `name`: The name of the module - if missing the default name is used
   * `path`: The path where the module can be fonud - if missing path is not
   provisioned

Limitations
----
Current known shortcomings of the module:

 * Tools cannot be (granularly) configured - only default config can be used
 * Only one box can be configured

Examples
----
The following is an example of a playbook that uses the `opensips_cp` role.
```
---
- hosts: all
  roles:
    - role: opensips_cp
```

Example of `opensips_cp_modules` variable:
```
opensips_cp_modules:
  system:
    modules:
      rtpproxy: RTPProxy
      monit: Monit
      smonitor: Statistics
```

Example of `opensips_cp_modules` specifying the module's path:
```
opensips_cp_modules:
  system:
    modules:
      monit: Monit
      smonitor: Statistics
  media:
    name: Media
    icon: images/icon-media.svg
    modules:
      rtpproxy:
        name: RTPProxy
        path: system/rtpproxy
```

License
----
GPLv3
