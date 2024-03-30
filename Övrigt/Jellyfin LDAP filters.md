# Fetch all users from the LDAP server
```
(objectClass=user)
```
# Sets one user as the admin for the jellyfin server 
**NOTE:** `Only one admin user`
```
(&(objectClass=user)(cn=username-or-email-here))
```