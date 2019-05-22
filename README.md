## Statistics Docker Hub

## Statistics Git Hub



# How to use Freeradius Server on Wago Device
## Prerequisites for tutorial
- Preinstalled SSH Client (e.g. https://www.putty.org/)
- Wago Device e.g. PFC200 G2 or Wago Touch Panel with minimal Firmware 12
  - Firmware you can find here: https://github.com/WAGO/pfc-firmware
  - Docker IPKG you can find here: https://github.com/WAGO/docker-ipk

### Starting the server
```$ docker run --name my-radius -d wagoautomation/pfc-freeradius-server```

The image contains only the default FreeRADIUS configuration which has no users, and accepts test clients on 127.0.0.1. In order to use it in production, you will need to add clients to the clients.conf file, and users to the "users" file in mods-config/files/authorize.

### Modify the configuration to yours

Where `clients.conf` contains a simple client definition
```bash
client YOUR-NAME {                 
    ipaddr = YOUR-NETWORK -> for example 172.17.0.0/16              
    secret = YOUR-SECRET -> for example testing123                   
}
```

and the `authorise` "users" file contains a test user:
```bash
YOUR-USERNAME    Cleartext-Password := "YOUR-USERPASSWORD"
```

Or you clone the [github repo](https://github.com/WAGO/pfc-freeradius-server) and modify the files `clients.conf` and `authorise` to your own. But then you need to use the volume mount flag, shown as follow:

```bash
$ docker run --name my-radius \   
-v $PWD/clients.conf:/etc/raddb/clients.conf \   
-v $PWD/authorize:/etc/raddb/mods-config/files/authorize \  
-d wagoautomation/pfc-freeradius-server
```

### How to use this images
 With standard configuration you can the image shwon as follow:
```bash
 $ docker run --name my-radius -p 1812-1813:1812-1813/udp wagoautomation/pfc-freeradius-server
```
For debbuging your configuartion you can use the `-X` attribut:
```bash
 $ docker run --name my-radius -p 1812-1813:1812-1813/udp wagoautomation/pfc-freeradius-server -X
```

### HowTo modify your Wago device for IEEE802.1x

Copy the file `wpa_supplicant.conf` from [github repo](https://github.com/WAGO/pfc-freeradius-server) to `/etc/` on your Wago device and modify it to yours.

Copy the file `wpa_supplicant` from [github repo](https://github.com/WAGO/pfc-freeradius-server) to `/etc/init.d/` and make a symlink with the following command.

```bash
ln -s /etc/init.d/wpa_supplicant /etc/rc.d/S97_wpa_supplicant
```
You can read the procedure in the wago cyber security manual for pfc controller.  [LINK cyber security manual](https://www.wago.com/de/d/15739)











