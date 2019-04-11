===========================
 Ansible Role: nginx_proxy
===========================

This role installs and configures Nginx with TLS certificates provided for free by LetsEncrypt, and provides mechanisms for configuring multiple virtual hosts with support for both reverse proxying to other services as well as statically served locations. Also supports basic auth for simple security access controls.

----------------
 Role Variables
----------------

Key role variables are documented with their default values below. See ``defaults/main.yml`` for a full list.

::

    admin_email: ""

LetsEncrypt uses this email address to send alerts about expiring certificates.

::

    nginx_proxy_sites: []

Each dictionary in this list defines a virtual host which will be created. The ``domain_name`` key should always be set and is the primary server name. ``aliases`` allows you to specify an optional list of alternate server names. ``internal_address`` is the address traffic to this virtual host should be forwarded to. ``locations`` is a list of dictionaries to directories which should be served as static sites. Finally, if ``auth_config`` is defined, it will require a user authenticate using Basic Auth before being allowed access.

------------------
 Example Playbook
------------------

::

    ---
    - hosts: localhost
      vars:
        admin_email: "admin@example.com"
        nginx_proxy_sites:
          - domain_name: www.example.com
            aliases:
              - app.example.com
            internal_address: internal.example.com:8080

          - domain_name: secure.example.com
            locations:
              - path: /www/secured/
            auth_config:
              description: "Login Required"
              contents: |
                admin:<password_hash>
      roles:
        - nginx_proxy
