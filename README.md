# FLSUN Speederpad USB Ethernet connection

For all who dont want to solder the RJ45 port to the PCB here is a work-a-round.

![alt text](https://github.com/Dentax/FLSUN-USB-Ethernet/blob/main/Adapter.jpg?raw=true)

## Requirements
- USB-Ethernet Adapter ***(actually it only supports RTL8152 chipsets because RTL8153 drivers are not available in the kernel by default)***
- SSH access to Speederpad
- ✨Magic ✨

# RTL8153 Chipsets
If your Adapter has an RTL8153 chipset you will need to add the drivers to the kernel.
Thanks to [attilabody](https://github.com/attilabody) for adding this missing drivers: https://github.com/attilabody/speederpad-kernel/releases/tag/v0.0.0-modules

After that you can try again with the following steps.

# Configuration
### Step 1
Connect to the Speederpad via SSH Client

### Step 2
Put the LAN cable in the adapter and router and connect it via USB to the Speederpad

### Step 3
On your SSH client type the followinng command:

```sh
ip link
```

now you should see the following output:

![alt text](https://github.com/Dentax/FLSUN-USB-Ethernet/blob/main/iplink.png?raw=true)

Copy the device name (red border) to a editor (e.g. Notepad)

### Step 4
Now we must configure the interface. The following command will open the configuration file:
```sh
sudo nano /etc/netplan/01-network-manager-all.yaml
```

the config should looks like this:

![alt text](https://github.com/Dentax/FLSUN-USB-Ethernet/blob/main/iplink01.png?raw=true)


### Step 5
There are two options for configuration

#### Static
Replace ***enx00e04c013511*** with the interface name from ***Step 3*** and change the IP-Address and Gateway
```
network:
        version: 2
        ethernets:
                enx00e04c013511:
                        dhcp4: no
                        optional: true
                        dhcp6: no
                        addresses: [192.168.1.123/24] # Speederpad IP
                        gateway4: 192.168.1.1 # Your Gateway IP 
                        nameservers:
                         addresses: [8.8.8.8,1.1.1.1]
```

#### DHCP
Replace ***enx00e04c013511*** with the interface name from ***Step 3***
```
network:
        version: 2
        ethernets:
                enx00e04c013511:
                        dhcp4: yes
                        optional: true
                        dhcp6: no
                        nameservers:
                         addresses: [8.8.8.8,1.1.1.1]
```

***Save the file (Command + X // STRG + X )***

### Step 6
Apply the config with the following command:
```sh
sudo netplan apply
```

### Step 7
Reboot the Speederpad over the touchpanel. Go to Configuration -> System -> System Restart

After the reboot the Speederpad is connected over the USB-Ethernet adapter 🎉

![alt text](https://github.com/Dentax/FLSUN-USB-Ethernet/blob/main/IP.jpg?raw=true)
