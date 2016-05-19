# Upgrading Lisk from Previous Version

## 1. Switch to your Lisk folder and Stop Lisk processes

```text
cd lisk
bash lisk.sh stop
```
	
## 2. Backup your config.json and ssl folder (if needed)

```text
mkdir ~/backup
cp config.json ~/backup/
cp ./ssl/* ~/backup/
```

## 3. Remove your old Lisk folder and install script

```text
cd ~
rm -rf ~/lisk
rm -rf installLisk.sh	
```

## 4. If you are upgrading from a release prior to 0.2.1 run these commands

```text
sudo apt-get --purge remove postgresql postgresql postgresql-client postgresql-client postgresql-client-common postgresql-common postgresql-contrib postgresql-contrib

rm -rf /var/lib/postgresql/
sudo rm -rf /var/log/postgresql/
sudo rm -rf /etc/postgresql/
```

## 5. Follow the Binary Install Guide!
	
https://lisk.io/documentation?i=lisk-docs/BinaryInstall
