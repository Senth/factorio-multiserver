# factorio-multiserver
Simple linux openrc service to easily update multiple Factorio servers including mods

## Installation Steps

1. Download zip file or clone this directory
1. Create user factorio if you haven't already with command `sudo useradd factorio`
1. Copy files
	1. etc/conf.d/factorio to /etc/conf.d/factorio
	1. etc/init.d/factorio to /etc/init.d/factorio
	1. opt/factorio to /opt/factorio
1. Download Factorio headless https://www.factorio.com/download-headless to /opt/factorio and rename the directory to server1 or another name you can more easily identify the server with.
1. Copy-paste the downloaded Factorio headless server for as many servers as you like.
1. For every server, go into the directory.
	1. Create an initial save using this commands `./bin/x64/factorio --create saves/my-save.zip --map-gen-settings data/map-gen-settings.json --map-settings data/map-settings.json`. Notice that you have to create your own `map-gen-settings.json` and `map-settings.json`, or if you want to use the default settings simply omit these parameters. See [Multiplayer Wiki](https://wiki.factorio.com/Multiplayer) for more information.
	1. Change the port in `config/config.ini` so you use different ports for every server. E.g., `port=34197` for first, `port=34198` for second, etc.
	1. Update service-username and service-token in `player-data.json` (found on the factorio website or in your own local `%appdata%\factorio\player-data.json` on Windows.)
1. Update /etc/conf.d/factorio file.
	1. Set `SERVERS` names (same name as the headless server directories in `/opt/factorio`)
	1. Set `OPTIONS`. Should be same amount of elements as servers
	1. Set `MODS_HOSTING_LOCATION` to a directory that you can share for easy downloading of mods. The zip files will be called `mods-[server_name].zip`
1. Install fac using this command `pip3 install --user -e "git+https://github.com/Senth/fac.git#egg=fac-cli"`

## Usage

### Server
```
sudo /etc/init.d/factorio start
sudo /etc/init.d/factorio stop
sudo /etc/init.d/factorio restart
sudo /etc/init.d/factorio update #Updates both server and mods
```

### Mods
To use fac you have to use it through the fac wrapper in `/opt/factorio/fac`. This wrapper automatically changes the config file of fac to point to the specified server. It also runs fac as the factorio user
```
fac server2 install auto-research
fac server1 help
```

## Limitations

* Due to openrc it's not possible to individually start or stop a specific server.
* All servers are updated to experimental version. Can be changed by introducing more options in the config file, but since I only use experimental version I didn't add that functionality. Feel free to fork and create a pull request :)

## Licenses
factorio-multiserver uses [fac](https://github.com/Senth/fac) for updating mods and [factorio-updater](https://github.com/narc0tiq/factorio-updater) for updating factorio servers
