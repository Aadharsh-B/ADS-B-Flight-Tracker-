# ADS-B Phase 2

Enabling ADS-B Flight Detection using dump1090-mutability.

## Suggested Reading

/home/raspberry/ADS-B Project/dump1090 Trixie OS/README.md

## Summary

To enable parsing of ADS-B Data as JSON | CSV, dump1090 is configured to read data from the RTL-SDR instead of rbfeeder.
rbfeeder in turn reads data from TCP of 127.0.0.1:30005.

Running the Station Site on Browser without Initializing dump1090?
Flights will not be visible, because flights are rendered from TCP data that is being served by the dump1090-mutability application, that reads data from the RTL-SDR.

ADS-B Signal -> Antenna -> RTL-SDR Serial Data -> dump1090-mutability -> {TCP, JSON}

It is therefore recommended to 

run

```bash
dump1090-mutability --net --net-bo-port 30005 --net-bind-address 127.0.0.1
```
If this throws an error run the command, for an occupied port, disable the mutability and re-run the above mentioned command:
```bash
sudo systemctl stop dump1090-mutability
```

before opening the flight tracker site `https://www.airnavradar.com/stations/EXTRPI690392` in browser.

## How to Obtain a Station?

To Recieve a Custom Station ID, run 
`sudo rbfeeder --showkey`

in the terminal to recieve a Sharing Key.

Paste the Key in :
`https://www.airnavradar.com/raspberry-pi/claim`
to recieve a Station ID.

### Explanation

To read TCP Data from the 127.0.0.1 30005 as defined in /etc/rbfeeder.ini

```ini
[network]
mode=beast
external_port=30005
external_host=127.0.0.1
```

Not enabling the TCP to Serve Data to `/stations/EXTRPI690392` results in a blank flight tracker site.

## Tip

Run the command to view options available in dump1090-mutability

`dump1090-mutability --help`

## dump1090 JSON?

Run
```bash
dump1090-mutability --net --net-bo-port 30005 --net-bind-address 127.0.0.1 --write-json <dir> --write-json-every 60
```

To send flight-tracker data to 127.0.0.1:30005 and also write data to JSON files in <dir>, every minute (60s) simultaneously.

### Significance

Data written as JSON can later be parsed using Python Code to generate a Permanent Log File.