# Installing Lisk (from Binaries)

This tutorial describes how to install the Lisk using pre-built binary packages.

To complete the installation, you will need to have `bash`, `curl`, `wget` and `tar` installed. The majority of operating systems have these installed by default, if not, please install them before continuing.

**NOTE:** With the release of **0.2.1**, PostgreSQL no longer needs to be installed separately. If you have installed it as part of a previous Lisk release, please stop it before continuing, e.g. on Ubuntu Linux do the following: **sudo service postgresql stop**.

## 1. Select Platform

The following operating systems and architectures are supported:

- [Linux (x86_64)](#linux-x86_64-)
- [Linux (i686)](#linux-i686-)
- [Linux (armv6l)](#linux-armv6l-)
- [Linux (armv7l)](#linux-armv7l-)
- [Darwin (x86_64)](#darwin-x86_64-)
- [FreeBSD (amd64)](#freebsd-amd64-)

If you are unsure which platform to choose from, open a terminal and run the following command:

```text
uname -sm
```

The resulting output, should tell you if your machine is running on a supported operating system and architecture.

If your architecture is not supported yet, you can try building your own packages using the [lisk-build](https://github.com/LiskHQ/lisk-build) automated package building tool.

## 2. Download Lisk

Follow the download instructions to install Lisk for your selected platform.

### All Platforms

1. Choose a network and download the appropriate install script:

  **Testnet** (_for development purposes_):

  ```text
  wget https://downloads.lisk.io/lisk/test/installLisk.sh
  ```

2. Execute the install script. This will download and install Lisk, configuring the environment for use.

  ```text
  bash installLisk.sh install
  ```

3. Change directory:

  ```text
  cd lisk
  ```

4. Configure environment _(optional, for apps development)_:

  ```text
  source env.sh
  ```

  Add this to your `.bash_profile` to make this change permanent:

  ```text
  echo "source $(pwd)/env.sh" >> ~/.bash_profile
  ```

5. Accessing Lisk

  To access the Lisk web client, open: [http://localhost:8000/](http://localhost:8000/) if on the mainnet (once Lisk is launched) or [http://localhost:7000/](http://localhost:7000/) if on a testnet, replacing **localhost** with your public IP address if you have one.

  The Lisk web client should launch successfully.
 
6. Proceed with the next step in this tutorial to [Enable Forging](#3-enable-forging).

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

## 4. Enable Secure Sockets Layer (SSL)

**NOTE:** To complete this step you require a signed certificate (from a CA) and a public and private key pair.

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

After you are done, save changes and exit. Hit: `Ctrl+ X` Then: `Y`

**NOTE:** If SSL Port configured above (ssl > options > port) is within well known ports range (below 1024), you must alter the port specified with setcap or change it to be outside of that range.

```text
sudo setcap cap_net_bind_service=+ep bin/node
```

Once this is done, or the port specified is above 1024, you may start Lisk

```text
bash lisk.sh start
```

Open the web client. You should now have an SSL enabled connection.

## 5. Available Commands

Listed below, are the available commands which can be used to manage your Lisk node.

To perform a coldstart (only needs to be performed once):

```text
bash lisk.sh coldstart
```

To stop/restart/start lisk:

```text
bash lisk.sh stop
bash lisk.sh start
bash lisk.sh restart
```

To reload config.json changes:

```text
bash lisk.sh reload
```

To check the status of lisk:

```text
bash lisk.sh status
```

To monitor the log file of lisk:

```text
bash lisk.sh logs
```

To replace the blockchain with a new snapshot:

```text
bash lisk.sh rebuild
```
