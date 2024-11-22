## Bind DN:
```
cn=ldapservice-account,ou=users,dc=ldap,dc=dc,dc=dc
```

## Bind credentials:
```
token/password
```

## User search base:
```
DC=ldap,DC=DC,DC=DC
```

## User search filter:
```
(cn={0})
```


## **NOTE:** Make sure the ldap service account have the following permissions in your provider

**Providers > your-ldap-provider > Permissions > User Object Permissions > Assign to new user**

![[emby-ldap-setup-perms.png]]**NOTE:** Then add these two Permissions

**TIP:** Make sure the ldap-service account is added to a group of the application that uses the ldap provider