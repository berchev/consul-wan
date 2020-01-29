# consul-wan

## Repo description
This repo provides Vagrant development environment containing simple configuration of 2 Consul Servers located in 2 different datacenters. Every Consul server has 2 Clients 

Some more details:
- Consul is configured as a systemd service.
- There is configured non-priviliged user called **consul**, which purpose is to run consul.
- Servers are configured with bootstrap enabled.
- Both clients have simple [web service](https://github.com/berchev/consul-wan/blob/master/scripts/client_service.sh) enabled 
- All nodes have syslog logging enabled
- All nodes have configured data directory and configuration directory
- Servers have **retry_join_wan** in order to accomplish communication between the datacenters

For more details related to Consul Server/Client configuration, you can check [server_provision.sh](https://github.com/berchev/consul-wan/blob/master/scripts/server_provision.sh) and [client_provision.sh](https://github.com/berchev/consul-wan/blob/master/scripts/client_provision.sh) scripts.

This project can be used as a fundamental step for other consul related project.

## Repo content
| File                   | Description                      |
|         ---            |                ---               |
| [Vagrantfile](Vagrantfile) | Vagrant template file. TFE env is going to be cretated based on that file|
| [scripts/server_provision.sh](scripts/server_provision.sh) | Consul Server provision script|
| [scripts/client_provision.sh](scripts/client_provision.sh) | Consul Client provision script|
| [scripts/client_service.sh](scripts/client_service.sh) | Consul Client web service creation script|


## Requirements
- VirtualBox installed
- Hashicorp Vagrant installed

## How to use this project
- clone the repo 
```
git clone https://github.com/berchev/consul-wan.git
```
- change to repo directory
```
cd consul-wan
```
- start provisioning of vagrant environment
```
vagrant up
```
- verify that all servers and client are in running status
```
vagrant status
```
- output should like this:
```
Current machine states:

server-dc1                running (virtualbox)
client1-dc1               running (virtualbox)
client2-dc1               running (virtualbox)
server-dc2                running (virtualbox)
client1-dc2               running (virtualbox)
client2-dc2               running (virtualbox)


This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
```
- ssh to node `server-dc1`
```
vagrant ssh consul-server1
```
- verify that both Servers are visible for each other
```
vagrant@server-dc1:~$ consul members -wan
Node            Address             Status  Type    Build  Protocol  DC   Segment
server-dc1.dc1  192.168.51.11:8302  alive   server  1.6.2  2         dc1  <all>
server-dc2.dc2  192.168.52.11:8302  alive   server  1.6.2  2         dc2  <all>
vagrant@server-dc1:~$ 
```
- you check the GUI interface for better visibility. Type `http://192.168.51.11:8500` in your favourite browser and will see very intuitive user friendly interface.

