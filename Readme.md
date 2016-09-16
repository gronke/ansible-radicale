Ansible Role: Radicale
======================

Provisions [Radicale](http://radicale.org/) - a simple calendar and contact server

Example
-------

### Radicale listening on IPv6 with Auth via LDAP (BindDN)

In this example the Radicale is listening on IPv6 tcp port 5232 on all interfaces. The rights file defines the access of non-anonymous users authenticated via LDAP.

```yaml
- role: gronke.radicale
  radicale_config:
    server:
      hosts: "[::]:5232"
      realm: 'Sign-in with your example.com account'
    auth:
      type: LDAP
      ldap_url: 'ldap://127.0.0.1'
      ldap_attribute: 'mail'
      ldap_base: 'ou=people,dc=example,dc=com'
      ldap_binddn: 'cn=radicale,ou=services,dc=example,dc=com'
      ldap_password: "correct horse battery staple"
      ldap_scope: OneLevel
    rights:
      type: from_file
      file: /etc/radicale/rights
```

#### /etc/radicale/rights
```ini
# user@example.com may read and write in /example.com/*
[domain-wide-access]
user: ^.+@(.+)\..+$
collection: ^{0}/.+$
permission: rw

# user-based share url: /share/a@example.com/b@examle.com/c@example.com/
[share]
user: .+
collection: ^share/.*%(login)s
permission: rw

# rw access to private calendars/contacts
[owner-write]
user: .+
collection: ^%(login)s/.*$
permission: rw
```

Compatibility
-------------

- Tested on Debian 8
