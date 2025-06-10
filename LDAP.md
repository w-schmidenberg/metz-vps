# Test Search

```sh
ldapsearch -x -H ldaps://ldap.<metz-domain.com>:636 \
-D "cn=ldap_bind_user,ou=ldap_search,dc=ldap,dc=<metz-domain>,dc=com" \
-b "dc=ldap,dc=<metz-domain>,dc=com" \
-w "{PASSWORD}" \
"(cn=test@metzcpa.com)"
```
