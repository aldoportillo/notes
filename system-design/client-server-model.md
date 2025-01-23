# The Client-Server Model

## Fundamental Knowledge

Client requests data from a server
Server sends requested data to client

## Browser to Website

Client doesn't know how to initially talk to the server. It makes a DNS query, a special request made to a predetermined set of servers that returns the IP of the requested domain, to find the IP address, a unique identifier for a machine, and finally the client can speak to the server.

If you'd like to see an example of the request an response use the dig command in your unix terminal.

```bash
dig github.com
```

After the IP address is received, the browser sends a HTTP request to the server.

## IP Addresses

An address given to each machine connected to the public internet.

- 127.0.0.1: Your own local machine. Also referred to as localhost.
- 192.168.x.y: Your private network. For instance, your machine and all machines on your private wifi network will usually have the 192.168 prefix.

## Ports

Any machine with an IP address contains 2<sup>16</sup> ports that programs on the machine can listen to.

So when communicating with a machine, you have to specify the port which you want to communicate on.

- Port 22 - Secure Shell
- Port 53 - DNS lookup
- Port 80 - HTTP
- Port 443 - HTTPS

## Netcat

A tool that allows you to read or write to network connections using certain protocols. Netcat will allow us to visualize communicating to an ip address.

In one terminal run:

```bash
nc -l 8081
```

This terminal is now listening on port 8081.

In another terminal run:

```bash
nc 127.0.0.1 8081
```

This command opens up a communication channel so whatever you type on this terminal will show on the terminal that is listening.

127.0.0.1 is an ip address that always points to your local machine.
Port 8081 is the port we are choosing to communicate on.
