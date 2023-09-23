# User creation

## Create a new user:
```bash
pveum user add example@pve
```

## Set or change the password (not all realms support this):
```bash
pveum passwd example@pve
```

## Disable a user:
```bash
pveum user modify testuser@pve -enable 0
```

## Create a new group:
```bash
pveum group add testgroup
```

# Administrator Group

## Define the group:
```bash
pveum group add admin
```

## Assign a role to the group:
```bash
pveum acl modify / -group admin -role Administrator
```

## Add users to the new _admin_ group:
```bash
pveum user modify example@pve -group admin
```
