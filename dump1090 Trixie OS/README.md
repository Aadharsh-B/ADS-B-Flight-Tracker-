# Intallation of FlightAware dump1090-mutability

dump1090-fa FlightAware's dump1090 is not available for installation on Trixie OS : Raspberry Pi.
Install for RPi 5 - 64 Bit ARM is available in the GitHub Repository : Install-dump1090-mutability-on-trixie.

[Repository Link](https://github.com/abcd567a/Install-dump1090-mutability-on-trixie/)

Install the Deb Package 'dump1090-mutability_1.15~20180310.4a16df3+dfsg-8.1_arm64.deb' available on the Repo, to install dump1090-mutability package.

## Verify Installation

Run 
```bash
raspberry@raspberry:~ $ dump1090-mutability
```

If installation is successful, a message from dump1090 appears.

Example

```bash
Sat Feb 14 13:29:00 2026 IST  EB_SOURCE EB_VERSION starting up.
Using sample converter: UC8, integer/table path
Found 1 device(s):
0: AIRNAV, ADSB_1090, SN: 00000010 (currently selected)
```

Install rtl-sdr for Enabling access to USB-Interface.

Run

```bash
sudo apt install rtl-sdr
```

Run

```bash
dump1090-mutability --interactive --net --write-json-every 1
```

Tests if dump1090 is successful in recieving data from the rtl-sdr.
Displays FlightData in Terminal, every 1s.

## If Error Appears?

Run

```bash
sudo systemctl stop rbfeeder
```

to terminate rbfeeder from accessing the rtl-sdr dongle serial port.

Run

```bash
dump1090-mutability --interactive --net --write-json-every 1
```

Tests if dump1090 is successful in recieving data from the rtl-sdr.
Displays FlightData in Terminal, every 1s.

## New rbfeeder Configuration

rbfeeder now recieves TCP data from 127.0.0.1 30005 for displaying realtime flights,
instead of directly recieving it from RTL-SDR serial port (This older configuration was a Part of ADS_B Phase-1).

```rbfeeder.ini
[network]
mode=beast
external_port=30005
external_host=127.0.0.1
```

ADS-B Signal -> Antenna -> RTL-SDR Serial Data -> dump1090-mutability -> {TCP, JSON}

### Advantages of Newer Configuration

```bash
dump1090-mutability --net --net-bo-port 30005 --net-bind-address 127.0.0.1 --write-json <dir>
```

can now be used to stream SDR-RTL serial data to TCP and Write to JSONs simultaneously.
