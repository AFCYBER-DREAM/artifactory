Artifactory
===========

The purpose of this role is to install Artifactory and ensure that the needed services are running. Post install configuration will need to be manually done.

Requirements
------------

Ensure that an up to date version of both this role and the postgresql role are available.

Role Variables
--------------

The default variables are stored in **defaults/main.yml** and are:

* packages_to_install:
* enabled_services:
* enabled_booleans:

Variables that should be set in either host or group vars are:

* db_server:
* db_password:

It is a good idea to use `ansible-vault encrypt_string` to encrypt the db_password variable.

Dependencies
------------

This role should be paired with the postgresql role so that the database server is set up properly prior to running this role.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - import_playbook: infrastructure.yml

    - name: Set up Artifactory
      hosts: artifactory
      become: True
      gather_facts: true

      roles:
        - teaming
        - nginx
        - postgresql
        - artifactory

Initial Post Provisioning Operations
------------------------------------

By default Artifactory encrypts the password that is stored in the db.properties file. This will cause the service to restart every time this role is used because it will see the encrypted password as a change. To get past this run the following curl once the activation key is set in the Web UI and the initial admin user is created to decrypt the password in the file. In the example curl the assumption is that the password for admin is **admin** but please use a stronger password instead. Also modify the URI as needed based on the network.

```bash
curl -u admin:admin -X POST http://$artifactory_uri/artifactory/api/system/decrypt
```

License
-------

MIT

Author Information
------------------

The Development Range Engineering, Architecture, and Modernization (DREAM) Team.
