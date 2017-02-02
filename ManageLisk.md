# Managing Lisk  
This document describes how to manage all three types of Lisk installations that was created using the Binary Installation

- [Managing a Binary Installation](#binary-installation)
- [Managing a Docker Installation](#docker-installation)
- [Managing a Source Installation](#source-installation)

## 1. Binary Installation

Listed below are the available basic commands which can be used to manage your Lisk node. For more detail, see each commands reference below.

```text
All options may be passed                -c <config.json>

start                                   Starts the Nodejs process and PostgreSQL Database for Lisk
stop                                    Stop the Nodejs process and PostgreSQL Database for Lisk
reload                                  Restarts the Nodejs process for Lisk
rebuild (-f file.gz) (-u URL) (-l) (-0) Rebuilds the PostgreSQL database
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

To rebuild from the genesis block:

```text
bash lisk.sh rebuild -0
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

The snapshot command is used to validate a copy of the blockchain from 0 and end gracefully to allow a backup to be taken. This function exists for use with `lisk_snapshot.sh`, but can be run adhoc as below. In the example, it snapshots to round 100, regardless of how much data exists beyond that round height.

```text
bash lisk.sh snapshot -s 100     
```

## 2. Docker Installation

# Determining docker instance ID

Get the docker container id:

```text
docker ps -a
```

The output from this command will be used in subsequent management entries.

# Start Docker

Start the docker container (replace **container_id** with your own id):

```text
docker start container_id
```

# Stop Docker

Stop the docker container (replace **container_id** with your own id):

```text
docker stop container_id
```

# Cycle Docker

Restart the docker container (replace **container_id** with your own id):

```text
docker restart container_id
```


## 3. Source Installation

Source installations for use in production are recommended to use a process manager. Forever is included in the installation document and will be used in this reference.

# Starting Source

Start a source installation with Forever.

```text
forever start app.js
```

# Stopping Source

Stop a source installation with Forever.
```text
forever start app.js
```

# Restarting Source

Restarts a source installation with Forever. This is useful for reloading changes to `config.json`

```text
forever restart app.js
```

# Rebuilding Source from Snapshot

In some scenarios its recommended to restore the blockchain from a snapshot. The command blocks below will perform this process. The url can be substituted for another `blockchain.db.gz` host if desired.


**MainNet**
```text
forever stop app.js
dropdb lisk_main
wget https://downloads.lisk.io/lisk/main/blockchain.db.gz
createdb lisk_main
gunzip -fcq blockchain.db.gz | psql -d lisk_main
forever start app.js
```

**TestNet**
```text
forever stop app.js
dropdb lisk_test
wget https://downloads.lisk.io/lisk/test/blockchain.db.gz
createdb lisk_main
gunzip -fcq blockchain.db.gz | psql -d lisk_test
forever start app.js
```

# View Realtime Logs

In order to view logs in realtime, use the following command. This is useful for troubleshooting issues.
```text
forever logs app.js -f
```
