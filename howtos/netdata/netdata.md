# Netdata

Net data is a statistics network data monitor, having a sleek webinterface.

## Installation

In order to update the package list, install-, and start the netdata monitor on the Turris Omnia:

- **root@turris:~#** `opkg update && opkg install netdata && netdata`

As a result Turris' Netdata will serve various statics on port 19999.

## Usage

Open your browser of choice and surf to the Omnia's address on port 19999:

- `http://192.168.1.1:19999`

To be presented with

![netdata example overview][2]

## References

Adapted from (Turris omnia forum): [Monitoring Omnia with NetData][1]

<!-- REFERENCES -->

[1]:https://forum.turris.cz/t/monitoring-omnia-with-netdata/3179/9
[2]:https://forum.turris.cz/uploads/default/original/2X/4/4585d87a776714b5769ad65eef8e034f560f041b.png
