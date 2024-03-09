# Bash Scripts!!!

This is a repo for scripts that I use to manage my server (hosted through Contabo). Not much here yet, and there probably won't be. It's just convenient to have so that I can port things later. Individual script documentation below:

## simplevpn

This is a wrapper script for running a wireguard vpn off of a server. Before running anything in the script, alter the HOSTNAME variable at the top of the simplevpn script file. This tells clients where the server is being hosted. I could have just gotten a public IP for the server in the script and directed the client to that, but I feel like a domain is better in case the server ever moves. You could still use an IP, but having a variable adds easier customizability to that.

For the actual script, it is pretty short and very easy to use.

The general structure for the command is: simplevpn <options> \[mode\] <arguments>

Adding -q as an option to any command silences the output. Other options and the arguments are mode dependent. Here are mode instructions:

All files produced by the program are put somewhere in /etc/wireguard.

### simplevpn -p x setup

This configures the server to run wireguard. It installs all the required libraries, creates a public/private key pair, and creates a wireguard configuration file.

The simplevpn network is configured for 10.0.0.0/24.

Specifying a -p option changes the port. It is 51820 by default.

### simplevpn -m addclient <name>

This adds a client to simplevpn with the given name. Configuration is placed in /etw/wireguard/configs as well as printed for copying to a wireguard app elsewhere. It also runs simplevpn restart so that the new client is loaded.

The -m option, if supplied, is for mobile support. It prints a QR code representation of the client configuration. The QR code does print, but I've gotten mixed results scanning it on the Android app, so the mileage may vary. I'm not going to try too hard to fix this because the configuration is still correct. 

### simplevpn teardown

This removes all simplevpn generated files and configurations.

### simplevpn status

This prints the systemctl status as well as the status returned by wg.

### simplevpn start | stop | enable | disable | restart

These commands all work exactly as systemctl does on a service because they are directly translated to systemctl commands that act on wireguard.
