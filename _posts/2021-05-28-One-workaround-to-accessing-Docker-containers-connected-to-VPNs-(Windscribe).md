---           
title: One workaround to accessing Docker containers connected to VPNs (Windscribe)
date: 2021-05-28 21:58:36 UTC
updated: 2021-05-28 21:58:36 UTC
comments: false
categories: howto docker
tags: docker vpn
---
Putting a docker container behind a VPN can be useful in many ways. For instance you can have a web service torrent client like transmission running in a docker container and not have your ISP throttle your speed after detecting where the high traffic is coming from... Or perhaps download files from sites that aren't available in your area. 

When I setup [a simple docker container](https://github.com/Kabe0/deluge-windscribe) with [Windscribe](https://windscribe.com/) (a VPN with 10gb free and torrent access) and deluge, I ran into the problem of the web GUI of deluge not being accessible from the mapped docker port when the VPN was enabled.

### Why this happens

My VPN Windscribe has a LAN-bypass feature. Usually you cannot access devices in your local network or be accessed by your network when connected to the VPN. LAN-bypass allows everyone in your subnet to access the server. Docker uses its own virtual network for all of its containers. Therefore the containers treat your local network (outside of the docker containers, IE your server, router, and computer) like WAN. 

  

Take my local network for example: it uses the 10.0.0.0/24 subnet while my docker containers use 172.17.0.0/16 subnet. So Windscribe starts and looks at the ethernet controller as if you were type in ifconfig. Let's say it sees 172.17.0.15. Windscribe now thinks that your local network is 172.17.0.0/16. The bypass only allows these IPs. Other containers can access this one as they are in the virtual docker network, but let's say my laptop (10.0.0.89) tries to access the forwarded port, it will time out. 

  

Basically Windscribe (or any other VPN) thinks that the docker VLAN is LAN and your actual LAN is WAN and therefore blocks traffic.

  

A good explanation is in [this serverfault answer.](https://serverfault.com/a/957949)

  

### Some possible solutions:

The first solution you can try is enabling LAN access or even disabling the firewall. In windscribe CLI you can do this by typing in the docker container's CLI: 

windscribe lan-bypass on 

windscribe firewall off

  

This did not work for me though. 

  

Issues may also occur with your VPN operating in the same subnet as your docker container, more information on this issue is in [this docker issue.](https://github.com/docker/for-mac/issues/2820) (the issue is for docker-mac, but could be applicable to other platforms)

  

Connecting the docker container's network to the host network is a possible solution, but can come with problems and will most definitely enable the VPN for your whole machine, which with a limited 10gb wouldn't be ideal for everyone.

  

###  What actually worked

This is a hack job solution, but here we go. Basically your network adapter on your host OS running docker will include a network adapter for the docker virtual network. The command ifconfig can bring up a list of network adapters. Usually the host OS's IP address is the subnet plus 1. So for 172.16.0.0/16 it would be 172.16.0.1. We can actually ping all the docker containers from the host OS! So going back to the example of the docker container having the address 172.17.0.15, if we ping that we should get a response. 

  
The solution here would be to get some sort of a web proxy if you don't already have one installed. I have apache web server installed, but nginx would work fine too ([probably better](https://www.reddit.com/r/sysadmin/comments/78scv6/nginx_or_apache/)). Add the proxy parameter to a virtualhost. This can be an issue especially if you are only working over LAN so you will need local DNS. In my case I finally ended up using jackett and transmission on the docker containers. I set my router running Fresh Tomato to define 10.0.0.30 (my server ip) to jackett.local and transmission.local. In apache config I put those two local addresses as my domain and pointed the proxy to 172.17.0.15:9117 and 172.17.0.15:9091. 

  

### Why this might not be the best solution

Docker IP addresses are very dynamic. For me they sometimes change every time I reboot, add, or delete a container. So you will have to either keep changing your proxy config or set a static IP. I set a static IP.

  
If it works it works.

  
I will edit this later. I didn't do too much research for this project yet, and will have to post issues in github, experiment, and read into this to solve the issue. I will hope to add detailed instructions soon, but for now, this is what I have.