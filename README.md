# wgkey
A simple CLI tool for managing a WireGuard proxy server

Requires WireGuard tools, Python and Python Click. For Ubuntu this can
be installed using `sudo apt install wireguard-tools python3-click`.
It also automatically sets up forwarding rules and is compatible with
UFW firewall if installed. The encryption keys will be automatically
generated as well.

## Getting Started
To setup the initial configuration, use the command:
```
sudo wgkey reset ENDPOINT PORT
```

`ENDPOINT` is the external ip or domain for your server so that the
clients can connect to the server.

`PORT` is the network port for the WireGuard server to listen on. You
will need to open up this port on the firewall and/or set up port
forwarding if your server is behind a NAT router.

You can add the following parameters to further configure the server.

`--prefix 10.0.0.` defines the ipv4 subnet for the clients. The
gateway for the clients will always be the prefix with a `1` at the
end, e.g. `10.0.0.1`. The client assigned ip addresses will then
start at `2` onwards.

`--prefix6 fd10:1010::` does the same as above for ipv6 addresses.

`--output eth0` defines the network interface the connected proxy
clients will use to reach the internet.

`--dns 10.0.0.1` sets the DNS server the clients should use when
connecting. If not included it will default to using the gateway
(which would be the server). The server should have a DNS service
such as `dnsmasq` running. You could instead use an external
service such as `1.1.1.1`

## Managing Clients
To add a client simply use:
```
sudo wgkey add NAME
```

`NAME` is the indentifier used purely within wgkey.

To generate the configuration to pass to the client machine use:
```
sudo wgkey configure NAME
```

You can view the existing clients, their public key and their IP using:
```
sudo wgkey list
```

You can remove existing clients from the server using:
```
sudo wgkey remove NAME
```

