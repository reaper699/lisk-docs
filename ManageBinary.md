# Managing Lisk Binary Install

This document describes how to manage a Lisk installation that was created using the Binary Installation

- [Managing the Lisk Installation](#accounts)
- [Forging Configuration](#lisk-api)
- [SSL Configuration](#introduction)


## 1. Basic Command Reference

Listed below are the available basic commands which can be used to manage your Lisk node. For more detail, see each commands reference below.

```text
All options may be passed                -c <config.json>

start                                   Starts the Nodejs process and PostgreSQL Database for Lisk
stop                                    Stop the Nodejs process and PostgreSQL Database for Lisk
reload                                  Restarts the Nodejs process for Lisk
rebuild (-f file.db.gz) (-u URL) (-l)   Rebuilds the PostgreSQL database
coldstart                               Creates the PostgreSQL database and configures config.json for Lisk
logs                                    Displays and tails logs for Lisk
status                                  Displays the status of the PID associated with Lisk
help                                    Displays this message
```


### Coldstart
```text
bash lisk.sh coldstart
```

### Start
To start Lisk and PostgreSQL:

```text
bash lisk.sh start
```

### Stop
To stop Lisk and PostgreSQL:

```text
bash lisk.sh stop
```

### Reload

To reload Lisk and pick up changes to config.json:

```text
bash lisk.sh reload
```

### Status

To check the status of Lisk:

```text
bash lisk.sh status
```

### Logs
To monitor(tail) the log file of Lisk:

```text
bash lisk.sh logs
```

### Rebuild

To replace the blockchain with a new snapshot from LiskHQ:

```text
bash lisk.sh rebuild
```

To use your own snapshot:

```text
bash lisk.sh rebuild -f blockchain.db.gz
```

To use your a remote hosts snapshot, must be named blockchain.db.gz:

```text
bash lisk.sh rebuild -u https://hostname/
```

To use a remote host snapshot with a different name issue this command instead:

```text
bash lisk.sh rebuild -u https://hostname/ -f filename.db.gz
```

## 2. Advanced command reference

Listed below are the available avanced commands which can be used to manage your Lisk node. For more detail, see each commands reference below.

```
All options may be passed                -c <config.json>

start_node                              Starts a Nodejs process for Lisk
stop_node                               Stops a Nodejs process for Lisk
start_db                                Starts the PostgreSQL database
stop_db                                 Stops the PostgreSQL database
snapshot -s ###                         Starts Lisk in snapshot mode
```

### Start Node

This command is used to start individual nodejs processes apart from the database. It is designed to be used with customized config.json files in order to manage vertically stacked Lisk processes on one node.

```text
bash lisk.sh start_node -c <config.json>
```

### Stop Node

This command is used to stop individual nodejs processes apart from the database. It is designed to be used with customized config.json files in order to manage vertically stacked Lisk processes on one node.

```text
bash lisk.sh stop_node   -c <config.json>
```

### Start Db

This command is used to start database instances apart from the Lisk process. It is designed to be used with customized config.json files to target specific instances.

```text
bash lisk.sh start_db   -c <config.json>
```

### Stop Db

This command is used to stop all database instances apart from the Lisk process.

```text
bash lisk.sh stop_db
```

### Snapshot

The snapshot command is used to validate a copy of the blockchain from 0 and end gracefully to allow a backup to be taken. This function exists for use with `lisk_snapshot.sh`, but can be run adhoc as below. In the example, it snapshots to round 10000, regardless of how much data exists beyond that round height.

```text
bash lisk.sh snapshot -s 10000     
```

## 3. Customizing config.json


## 3. Enable Forging

If you are running your node from a local machine, you can enable forging through the web client, without further interruption. **NOTE:** Should the Lisk node or machine need to be restarted, you will need to re-enable forging again.

If your node is running on a remote machine, or if you want to keep forging persistently enabled, you will need to follow the below instructions.

Stop the running Lisk node:

```text
bash lisk.sh stop
```

Open config.json:

```text
nano config.json
```

Arrow down until you find the following section:

```text
"forging": {
  "secret" : [""]
}
```

Set the secret parameter to your account secret phrase.

```text
"forging": {
  "secret" : ["YourDelegatePassphrase"] <- Replace with your delegate passphrase
}
```

(Optional) In the forging section you will also see an access property, this is used to allow only your IP address to enable forging through the web client.

```text
"access": {
  "whiteList": ["127.0.0.1"] <- Replace with the IP which you will use to access your node
}
```

To set 2 accounts to forge on a single node, enter both account passphrases like below.

```text
"forging": {
  "secret" : ["YourDelegatePassphrase1","YourDelegatePassphrase2"] <- Replace with your delegate passphrases
  "access": {
    "whiteList": ["127.0.0.1"]
  }
}
```

After you have typed in your passphrase. Hit: `Ctrl+ X` Then: `Y`

Start Lisk:

```text
bash lisk.sh start
```

Then, open the Lisk web client and wait for the blockchain to load. Once the blockchain has loaded, navigate to "Forging" section, and verify that **Forging (Enabled)** appears in the top left corner.

## 3. Enable Secure Sockets Layer (SSL)

**NOTE:** To complete this step require a signed certificate (from a CA) and a public and private key pair.

Stop the running Lisk node:

```text
bash lisk.sh stop
```

Open config.json:

```text
nano config.json
```

Arrow down until you find the following section:

```text
"ssl": {
  "enabled": false,         < Change FROM false TO true
  "options": {
    "port": 443,            < Default SSL Port
    "address": "0.0.0.0",   < Change only if you wish to block web access to the node
    "key": "path_to_key",   < Replace FROM path_to_key TO actual path to key file
    "cert": "path_to_cert"  < Replace FROM path_to_cert TO actual path to certificate file
  }
}
```

After you have finished, save changes and exit. Hit: `Ctrl+ X` Then: `Y`

**NOTE:** If SSL Port configured above (ssl > options > port) is within well known ports range (below 1024), you must alter the port specified with setcap or change it to be outside of that range.

```text
sudo setcap cap_net_bind_service=+ep bin/node
```

Once this is done, or the port specified is above 1024, you may start Lisk

```text
bash lisk.sh start
```

Open the web client. You should now have an SSL enabled connection.
