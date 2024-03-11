# Welcome In!

You have reached the internal documentation for tialsinn, an ubuntu server (no one remembers what tialsinn stands for). The server is hosted by Contabo, which has so far been a very pleasant experience.

This documentation serves 3 purposes:

1. If you are using the server, this guide explains what is running on the server, why, and what tools are available for those programs. From the command line, serverhelp will bring you here
2. If the server ever has to be moved or somehow is destroyed, this documentation will explain most everything that has been done with the server and all attached machines to get it up and running again
3. If you are looking to hire me (please) or just looking around my github, this is a very very very detailed README that explains this server and the utilities I've written for it

## Tialsinn on the Web!

This section is a short description of the externally facing services running on tialsinn. The server is running UFW and only has a few ports externally open, so they will be explained here:

Ports 22 and 2222 are open for SSH traffic. The ssh server is fairly standard: only keys are allowed as authentication methods, and each user on the system can have a set of authorized keys

Ports 80 and 443 are open for web traffic through NGINX. It won't be very long until the Nginx configuration hell needs its own section, but for right now the server isn't doing too much.

Ports 53 and 853 are open for DNS and DNSSEC traffic. Port 53 was running a DNS server that returned 0.0.0.0 for any reddit or youtube lookups, but the solution I had was slow. More to come there. 853 is still open and just uses nginx to proxy back to the server running on 53. This will work great when there is a server running on 53.

Ports 6900:6999 are open for testing if I ever need that. There won't usually be anything coming out of there.

Port 25 is open for mail, but I don't have a good enough understanding of SMTP to write much more thatn that. More there as well.

Port 25565 is open for minecraft servers when those are running.

Port 51820 is open for wireguard traffic. Tialsinn is primarily a VPN, managed by the simplevpn script (documentation in the server scripts section).

## Server Scripts

This section contains documentation for scripts that have been written specifically for this server. These scripts (as well as this whole repository) are currently cloned to /servertools, so they can all be accessed by adding the directory to path. They all have read and execute permission for everyone.

### serverhelp

This is a script that gives server help! There are no arguments or options; serverhelp gives serverhelp. It just prints this file rn, but it will be smarter soon.

### simplevpn

This is a wrapper script for running a wireguard vpn off of a server. Before running anything in the script, alter the HOSTNAME variable at the top of the simplevpn script file. This tells clients where the server is being hosted. I could have just gotten a public IP for the server in the script and directed the client to that, but I feel like a domain is better in case the server ever moves. You could still use an IP, but having a variable adds easier customizability to that.

For the actual script, it is pretty short and very easy to use.

The general structure for the command is: simplevpn <options> \[mode\] <arguments>

Adding -q as an option to any command silences the output. Other options and the arguments are mode dependent. Here are mode instructions:

All files produced by the program are put somewhere in /etc/wireguard.

#### simplevpn -p x setup

This configures the server to run wireguard. It installs all the required libraries, creates a public/private key pair, and creates a wireguard configuration file.

The simplevpn network is configured for 10.0.0.0/24, but it is not started directly after configuration. simplevpn start must be run

Specifying a -p option changes the port. It is 51820 by default.

#### simplevpn -m addclient <name>

This adds a client to simplevpn with the given name. Configuration is placed in /etw/wireguard/configs as well as printed for copying to a wireguard app elsewhere. It also runs simplevpn restart so that the new client is loaded.

The -m option, if supplied, is for mobile support. It prints a QR code representation of the client configuration that can be scanned by the wireguard app. 

#### simplevpn teardown

This removes all simplevpn generated files and configurations.

#### simplevpn status

This prints the systemctl status as well as the status returned by wg.

#### simplevpn start | stop | enable | disable | restart

These commands all work exactly as systemctl does on a service because they are directly translated to systemctl commands that act on wireguard.
