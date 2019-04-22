# Deploy my k3s cluster with Ansible

## Purpose

This ansible playbook allow me to deploy a k3s (v0.4.0) cluster on my raspberry pi cluster running on raspbian. This cluster has one master and 3 slaves.

It deploys the openfaas-figlet function in the cluster with two replicas in order to check that everything is working fine.

This cluster allow me to learn about k8s, I'm not using it in "production".

## What is done in this playbook

### playbook-k3s.yml

1. Use raspi-config in order to configure the memory-split, timezone, hostname and expand file system.
2. Activate cgroup on raspbian.
3. Install the squid-deb-proxy-client and update the nodes. I have not a very good internet connection speed, that's why I deployed a squid-deb-proxy on my server at home.
4. Deploy and configure ufw.
5. Install my personal configuration and aliases. It's a bit useless, but I like to have my home configured.
6. Download, deploy and configure k3s as a service both on master and slave nodes.
7. Deploy the openfaas-figlet function in the cluster

### playbook-shutdown.yml

It just shutdowns the cluster.

## Test the playbook

In order to test this playbook, you will have to update the host.ini file to match your ip address. Then run the playbook.

``` shell
[0][2019-04-22 09:39:10][348][florentclarret@dana:~/Documents/Dev/Ansible/pi-k3s-cluster] (branch: master)
 $ ansible-playbook playbook-k3s.yml
```

If everything is fine (it is on my side), you can hit the openfaas function :

``` shell
[0][2019-04-22 09:50:39][1][florentclarret@dana:~/Documents/Dev/Ansible/pi-k3s-cluster] (branch: master)  
 $ echo -n "k3s" | curl --data-binary @- http://10.0.5.100:31111
 _    _____     
| | _|___ / ___ 
| |/ / |_ \/ __|
|   < ___) \__ \
|_|\_\____/|___/
                
```

## Hardware

My cluster is composed of 3 RPi 3B+ and 1 RPi 3B. I'm currently using a netgear switch and an anker power supply.

## Related links

This playbook build what Alex Ellis describes in this article: [Will it cluster? k3s on your Raspberry Pi](https://blog.alexellis.io/test-drive-k3s-on-raspberry-pi/ "Will it cluster? k3s on your Raspberry Pi").

Also, Vincent RABAH already made an ansible playbook for k3s, which is available [on his github](https://github.com/itwars/k3s-ansible).
