Ansible Role: Keycloak
=========

An Ansible Role that installs et configure [Keycloak](https://www.keycloak.org/) on Linux.

Requirements
------------

You need:
 - At least OpenJDK 11 or newer installed before running this role. To ensure it's installed, you can use the `geerlingguy.java` role for example.
 - The `sudo` command and a user allowed to use it (or `root` user)

And requirements to [run Keycloak in production](https://www.keycloak.org/server/configuration-production):
 - A [supported database](https://www.keycloak.org/server/db)
 - A valid [hostname](https://www.keycloak.org/server/configuration-production)
 - [HTTPS/TLS certificates](https://www.keycloak.org/server/db) provided in PEM format or in a Java Keystore

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    keycloak_version: 20.0.2

The version of Keycloak to install on the system. If not defined, we try to find the latest release on GitHub API.

    keycloak_install_dir: /opt/keycloak

The target folder where Keycloak version will be installed.

    keycloak_hostname: keycloak.example.com

Hostname for the Keycloak server. If not specified, `ansible_fqdn` variable will be used.

    keycloak_https_port: 8443

The port listened by Keycloak for HTTPS traffic.  
WARNING: As Keycloak will run as a service user (non-root), it cannot be < 1024. 
See [Keycloak documentation](https://www.keycloak.org/server/reverseproxy) to run Keycloak behind a proxy if you need. 

    keycloak_cert_file: /etc/ssl/certs/example/example.com.crt
    keycloak_key_file: /etc/ssl/private/example.com.key

The file path to SSL/TLS certificate and associated key (ignored if using a Java Keystore).  
Please note that for now, there's no way to provide a passphrase for the key to Keycloak.

    keycloak_keystore_file: /opt/keycloak/keycloak.jks
    keycloak_keystore_password: changeme

The file path and password for the Java Keystore containing the SSL/TLS certificate and associated key.

    keycloak_service_user: keycloak

The system user/group name who will be running Keycloak. The role will automatically create & set up the user account if necessary.

    keycloak_database_host: localhost
    keycloak_database_vendor: postgres
    keycloak_database_user: db_user
    keycloak_database_password: db_password
    keycloak_database_name: keycloak_db

Settings for the database to use with Keycloak. See [Keycloak documentation](https://www.keycloak.org/server/all-config#_database) for details on allowed values.



Dependencies
------------

None

Example Playbook
----------------

```yaml
- hosts: all
  roles:
    - role: sedona_solutions.keycloak
      vars:
        keycloak_version: 20.0.1
        keycloak_keystore_file: /etc/keycloak/keycloak.jks
        keycloak_keystore_password: changeme
        keycloak_hostname: keycloak.example.com
        keycloak_database_host: localhost
        keycloak_database_vendor: postgres
        keycloak_database_user: db_user
        keycloak_database_password: db_password
        keycloak_database_name: keycloak_db
```

License
-------

MIT / BSD

Author Information
------------------

This role was created in 2022 by [Sébastien Collado](https://github.com/scollado) for [Sedona Solutions](https://github.com/Sedona-Solutions).
