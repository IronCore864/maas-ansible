
# 0 Prerequisites

A MAAS server running.

In order to access MAAS via CLI, an API key needs to be fetched. It is created when you create the admin user.

Fetching the API key by the following command:

```
sudo maas-region apikey --username=admin > /home/tiexin/maas_api_key
```

Then you need to do MAAS login

```
#!/bin/bash
# Change these 3 values as required
PROFILE=admin
API_KEY_FILE=/home/tiexin/maas_api_key
API_SERVER=localhost

MAAS_URL=http://$API_SERVER:5240/MAAS/api/2.0

maas login $PROFILE $MAAS_URL - < $API_KEY_FILE
```

This handy short shell script will handle the login easier for you.

# 1 Nodes/hosts

```
maas $PROFILE nodes read 
```

This command returns all the information about the nodes.

```
maas $PROFILE nodes read | grep hostname
```

With this command you can fetch all the hostnames only.

# 2 SYSTEM ID

```
# set hostname
HOSTNAME=useful-racer
SYSTEM_ID=$(maas $PROFILE nodes read hostname=$HOSTNAME | grep system_id -m 1 | cut -d '"' -f 4)
```

This command gets the SYSTEM_ID for the specified host.

# 3 Commission

```
maas $PROFILE machine commission $SYSTEM_ID
```

This command does commission for a certain host but it works using SYSTEM_ID instead of hostname.

# 4 Acquire/allocate

```
maas $PROFILE machines allocate system_id=$SYSTEM_ID
```

Acquiring a node (sometimes called "allocating" a node) is simply a means of reserving the node so that it is no longer available to any other process, whether that process be MAAS itself (e.g. another MAAS user) or a process such as Juju that uses MAAS as its source of backing machines.

Before a node is deployed it must therefore always be acquired, resulting in a status of 'Allocated'.

This is not needed in the manual step mentioned in the other document 02 - MAAS - Installation and Local Test with VirtualBox because when deploying from the web UI this action is performed automatically (and invisibly).

# 5 Deploy

```
maas $PROFILE machine deploy $SYSTEM_ID
```

# 6 Tag

```
maas $PROFILE tags read
```

List all the tags

```
maas $PROFILE tags create name=$TAG_NAME comment='$TAG_COMMENT'
```

Create one tag

```
maas $PROFILE tag update-nodes $TAG_NAME add=$SYSTEM_ID
```

Manually tag a node, probably useful for ansible dynamic inventory.

# Todo:

Images, source, import, specify specific image to deploy.

