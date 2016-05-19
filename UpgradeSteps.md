#### Upgrading Lisk from Previous Version

1. Switch to your lisk folder

[code]	cd lisk[/code]
	
2. Stop lisk processes

[code]	bash lisk.sh stop[/code]
	
3. Backup your config.json and ssl folder (if needed)

[code]	mkdir ~/backup
	cp config.json ~/backup/
	cp ./ssl/* ~/backup/
	[/code]

4. Remove your old Lisk folder and install script

[code]
cd ~
rm -rf ~/lisk
rm -rf installLisk.sh[/code]	


5. If you are upgrading from a release prior to 0.2.1 run these commands

[code]sudo apt-get --purge remove postgresql postgresql postgresql-client postgresql-client postgresql-client-common postgresql-common postgresql-contrib postgresql-contrib

rm -rf /var/lib/postgresql/
sudo rm -rf /var/log/postgresql/
sudo rm -rf /etc/postgresql/[/code]

6. Follow the Binary Install Guide!
	
[url]https://lisk.io/documentation?i=lisk-docs/BinaryInstall[/url]
