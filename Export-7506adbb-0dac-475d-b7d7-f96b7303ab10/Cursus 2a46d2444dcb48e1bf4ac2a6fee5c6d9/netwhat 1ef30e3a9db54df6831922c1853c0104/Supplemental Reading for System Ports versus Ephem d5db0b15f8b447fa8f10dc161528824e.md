# Supplemental Reading for System Ports versus Ephemeral Ports

## 

---

Transportation layer protocols use a concept of ports and multiplexing/demultiplexing to deliver data to individual services listening on network nodes. These ports are represented by a single 16-bit number, meaning that they can represent the numbers 0-65535.

This range has been split up by the [IANA](https://www.iana.org/) (**Internet Assigned Numbers Authority**) into independent sections:

Port 0 isn’t in use for network traffic, but it’s sometimes used in communications taking place between different programs on the same computer.

Ports 1-1023 are referred to as **system ports,** or sometimes as “**well-known ports**.” These ports represent the official ports for most well-known network services. In an earlier video, we talked about how HTTP normally communicates over port 80, while FTP usually communicates over port 21. In most operating systems, administrator-level access is needed to start a program that listens on a system port.

Ports 1024-49151 are known as **registered ports**. These ports are used for lots of other network services that might not be quite as common as the ones that are on system ports. A good example of a registered port is 3306, which is the port that many databases listen on. Registered ports are sometimes officially registered and acknowledged by the IANA, but not always. On most operating systems, any user of any access level can start a program listening on a registered port.

Finally, we have ports 49152-65535. These are known as **private or ephemeral ports**. Ephemeral ports can’t be registered with the IANA and are generally used for establishing outbound connections. You should remember that all TCP traffic uses a destination port and a source port. When a client wants to communicate with a server, the client will be assigned an ephemeral port to be used for just that one connection, while the server listens on a static system or registered port.

Not all operating systems follow the ephemeral port recommendations of the IANA. In this lesson, we’ll continue to assume that the ephemeral ports used for outbound connections consist of the ports 49152 through 65535. But it’s important to know that this exact range can vary depending on the platform you’re working on. Sometimes portions of the registered ports range are used, but no modern operating system will ever use a system port for outbound communication

To learn more about ports, and to see a list of what ports have been assigned to what services, check out the IANA [Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers). A similar list [on Wikipedia](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers) is not official, but it is a little easier to read. Check it out, too!