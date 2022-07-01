# Install the Opencast Integration for BigBlueButton

![linter](https://github.com/elan-ev/bbb_oc_integration/actions/workflows/linting.yml/badge.svg)

This is an ansible-role to set up the [opencast-bigbluebutton-integration](https://github.com/elan-ev/opencast-bigbluebutton-integration)
to work with exisiting [BigBlueButton](https://bigbluebutton.org/) and [Opencast](https://opencast.org/) installations.

This role can be used to configure the respective BigBlueButton servers and install all the necessary components.
For the Opencast instance you still need to create the respective workflow and the users.
However, this is usually just copying the workflow file to the correct destination on your installation.
You can find examples of valid workflow files here: https://github.com/elan-ev/opencast-bigbluebutton-integration/tree/master/post-archive

## Requirements

This role assumes that the file `/etc/bigbluebutton/bbb-web.properties` is already present (if not you need to create it beforehand).

## Role Variables

### Required Variables

There are some required variables to be able to connect to opencast.
Without them this role will not work.
They are:

| Variable | Example Value | Description |
|:--|:--|:--|
| `bbb_oc_workflow` | `post-archive` or `post-publish` | The workflow to use |
| `bbb_oc_opencast_server` | `https://develop.opencast.org` | The opencast server that ingests the recordings |
| `bbb_oc_opencast_user` | `admin` | The user that can access opencast  |
| `bbb_oc_opencast_password` | `your-password` | The password for that user |

### Version Checkout

The role uses the [`ansible.builtin.git`-module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/git_module.html)
to download the ruby scripts for the integration from [their repository](https://github.com/elan-ev/opencast-bigbluebutton-integration).
You can checkout the desired version with the `bbb_oc_version` variable that is directly passed to the module's parameter [`version`](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/git_module.html#parameter-version). The **default** for this variable is `HEAD`.

### Optional Variables

There are further variables you can use to configure the integration.
For a full overview of all the configuration options have a look at the [defaults](defaults/main.yml)-file.
For a better understanding you should probably also have a look at the configuration of the
[opencast-bigbluebutton-integration](https://github.com/elan-ev/opencast-bigbluebutton-integration)
and of course BigBlueButton and Opencast themselves.

## Example Playbook

Your playbook might look like this:

```yaml
---

- hosts: all
  become: true
  vars:
    bbb_oc_workflow: post-archive
    bbb_oc_opencast_server: https://develop.opencast.org
    bbb_oc_opencast_user: admin
    bbb_oc_opencast_password: '{{ your_password_from_vault }}'
  roles:
    - role: elan.bbb_opencast_integration
```

## Development

For linting and role development you can use the tools defined in [development requirements](.dev_requirements.txt).
You can install them in a python virtual environment like this:

```sh
# Create a virtual environment
python -m venv venv
# Activate the virtual environment
. venv/bin/activate
# Install the dependencies
pip install -r .dev_requirements.txt
```

E.g. you can then run the linter (`yamllint -c .yamllint .`) or test your setup with `molecule`.

## License

[BSD-3-Clause](LICENSE)

## Author Information

[ELAN e.V](https://elan-ev.de/)
