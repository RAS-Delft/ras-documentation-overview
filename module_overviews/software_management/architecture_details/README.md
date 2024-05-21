# RAS fleet stack software architecture details
This section elaborates which and how we deploy software across our systems.

Commonly a drone has the following devices upon which we deploy software:
- 1 main microcontroller that interacts with actuator and analog sensors.  (e.g. Arduino, ESP, ...)
- 1 linux based pc with more conputational power, albeit not designed to directly interact with analog systems. (e.g. NUC, Raspberry-Pi ...) 


This image sketches the 2024 vision on structuring drone subsystems
<br>

![ras_system_architecture_workspace drawio](https://github.com/RAS-Delft/ras-documentation-overview/assets/5917472/eadc34ec-44f6-4790-a78f-63b8fbfa0c5d)


## Drone Computational hub
This device is the computer on board of the vessel has the main task of linking ROS with actuators/sensors. Additionally it provides diagnostics and can execute automated computation locally if desired. 

### Benchmark OS setup
- Install a recent linux distribution. You can choose a more minimalistic OS, or one with conveniencies such as a desktop GUI. 
    - For Raspberry Pi's consider using the linux raspberry-pi-imager program from the software center. It's quite convenient. I used Ubuntu-Server-24.04LTS64bit image for RPI4B
    - Consider what this distro is compatible with. Docker can give you flexibility in deploying modules, but if you want to run something specific, check if it is supported (e.g. check support for your envisioned ros distibution).
- Configure the network to connect to RAS routers or VPN. For RPI4 server I changed the network-config on the sdcard to know the NETGEAR21 router. 
- Configure SSH connection. Make sure you can ssh with a terminal from another PC to the device using `ssh defaultusername@192.168.1.3` (default user is commonly 'ubuntu' or 'pi' depending on the OS you chose) [This guide](https://phoenixnap.com/kb/ssh-permission-denied-publickey) worked nice for me. 
- Set username to ras with new password. 
    - SSH into device using default user ssh ubuntu@192.168.1.3
    - add ras user `sudo adduser ras`
    - add ras user to sudo group `sudo adduser temporary sudo`
    - `exit` and relog as new user `ssh ras@192.168.1.3`
    - remove default user `sudo deluser ubuntu` and its homefolder `sudo rm -r /home/ubuntu`
- Change the device name to something sensible: `sudo nano /etc/hostname` and preferably unique (to allow login by name later)
- (optional) Enable login by hostname by `sudo nano /etc/systemd/resolved.conf` and setting `MulticastDNS=yes`
- (optional) Set access keys between device and your pc for easy access: `ssh-copy-id ras@192.168.1.3`

### Benchmark module setup
All major components are stored in the ras homefolder (`/home/ras`).
- Add the configuration file for this drone, where we set the major settings for this device.
    - `sudo nano /home/ras/ras_entrypoint.sh' and edit the fields as required
``` 
#!/bin/bash
export VESSEL_ID="RAS_TN_DB"
export RAS_GH_USERNAME="ras-delft-user"
export RAS_GH_KEY="<FILL IN ras-delft-user pull/write GITHUB KEY>"
```
    - Make sure this gets called upon bash startup by adding it to .bashrc with `nano` or `echo "source /home/ras/ras_entrypoint.sh" >> ~/.bashrc`
- Get the repository with main dockerfiles
    - `git clone https://${RAS_GH_USERNAME}:${RAS_GH_KEY}@github.com/RAS-Delft/ras_ros_low_level_systems`

## Microcontroller
This device is responsible for executing desired actuation. 


## Design principles

### Abstraction
Software components should be reused if possible. Elements that are comparable between different vessels/model lines are to come from a single source, to be customized through vessel specific configuration files. Examples of this are: Ros-micro bridge, main microcontroller code. 

### Modularity
Components should be swapped out if reasonable, taking into account complexity, effort, latency and so on. This allows flexibility and usage of external modules. We envision this through:
- Using ROS
- Use standardized interfaces, topics, coordinate systems, topology.

### Deployment
- Control systems in ROS are preferred to be rospackages set up to be included in launchfiles. 
- Containerize elements (commonly with Docker) for flexible deployment
- Version control with Git
- Document

## Common utilities:
- [OvenMediaEngine](https://github.com/AirenSoft/OvenMediaEngine) container for streaming video
- [Traefic](https://hub.docker.com/_/traefik) for remote deployment of services 
