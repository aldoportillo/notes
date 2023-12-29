# Network Protocols

A protocol is an agreed upon set of rules for an interaction between two parties.

## IP - Internet Protocol

When a machine tries to communicate with another machine that data is going to be sent in the form of an IP packet which is made up of bytes.

An IP packet has two parts:

- Header part is about 20 to 60 bytes and it contains information about the packet: the source IP address, the destination IP address, total packet size, IP version (IPv4 or IPV6).
- Payload has a maximum size of 2<sub>16</sub> bytes. So we need to send multiple packets. 

## TCP - Transmission Control Protocol

Since an IP packet's payload is limited to 2<sub>16</sub> bytes, we need to send multiple packets. The problem is some packets might be missing or they might be sent in the wrong order.

TCP solves this. TCP ensures IP packets are sent in an ordered and reliable way.

In order for TCP to work, there needs to be a TCP header and that lives in the payload of the IP packet and it contains information about the order.

If a browser wants to communicate with a server it first establishes a TCP connection.

This is done via the TCP IP handshake:

- Client sends a couple packets to server to request a connection.
- Server approves the connection
- Client verifies connection

Either machine can end the connection.

## HTTP - Hyper Text Transfer Protocol

