# ADS-B Flight Tracker Project

**RTL-SDR based 1090 MHz Aircraft Tracking System**

This project configures a **1090 MHz ADS-B receiver using RTL-SDR**, processes aircraft signals using **`dump1090-mutability`**, sends the data to **AirNav Radar**, and optionally **logs flight data locally as JSON/CSV for later analysis**.

---

# 1. System Overview

### Signal Processing Pipeline

```
ADS-B Signal
     ↓
1090 MHz Antenna
     ↓
RTL-SDR Dongle (USB)
     ↓
dump1090-mutability
     ↓
TCP Data + JSON Output
     ↓
rbfeeder
     ↓
AirNav Radar Server
```

### Key Roles

| Component               | Purpose                           |
| ----------------------- | --------------------------------- |
| **RTL-SDR**             | Receives raw ADS-B radio signals  |
| **dump1090-mutability** | Decodes aircraft data             |
| **rbfeeder**            | Sends decoded data to AirNav      |
| **AirNav Radar**        | Displays flights on web interface |

---

# 2. Project Directory

Main project directory:

```
/home/raspberry/ADS-B Project/
```

Important files:

```
dump1090 Trixie OS/README.md
Documentation/
```

Documentation folder includes:

```
Raspberry Pi RBFeeder Guide.pdf
RBfeeder on Raspberry Pi OS Trixie.pdf
```

---

# 3. Installing dump1090-mutability

FlightAware’s `dump1090-fa` **is not available for Raspberry Pi OS Trixie (64-bit)**.

Instead use:

**GitHub Repository**

```
https://github.com/abcd567a/Install-dump1090-mutability-on-trixie/
```

Download and install the package:

```
dump1090-mutability_1.15~20180310.4a16df3+dfsg-8.1_arm64.deb
```

Install using:

```bash
sudo dpkg -i dump1090-mutability_1.15~20180310.4a16df3+dfsg-8.1_arm64.deb
```

---

# 4. Install RTL-SDR Drivers

Install required RTL-SDR drivers:

```bash
sudo apt install rtl-sdr
```

This allows the Raspberry Pi to access the **RTL-SDR USB device**.

---

# 5. Verify dump1090 Installation

Run:

```bash
dump1090-mutability
```

Expected output example:

```
Sat Feb 14 13:29:00 2026 IST EB_SOURCE EB_VERSION starting up.
Using sample converter: UC8, integer/table path
Found 1 device(s):
0: AIRNAV, ADSB_1090, SN: 00000010 (currently selected)
```

This confirms:

* RTL-SDR detected
* dump1090 running correctly

---

# 6. Test ADS-B Signal Reception

Run:

```bash
dump1090-mutability --interactive --net --write-json-every 1
```

This will:

* display aircraft in **terminal**
* refresh data every **1 second**

---

# 7. If an Error Appears

If the RTL-SDR device is busy:

Stop `rbfeeder` temporarily.

```bash
sudo systemctl stop rbfeeder
```

Then run again:

```bash
dump1090-mutability --interactive --net --write-json-every 1
```

---

# 8. Running dump1090 for Flight Tracking

Run the following command:

```bash
dump1090-mutability --net --net-bo-port 30005 --net-bind-address 127.0.0.1
```

Purpose:

* Reads data from RTL-SDR
* Sends decoded aircraft data to:

```
127.0.0.1:30005
```

This TCP port is used by **rbfeeder**.

---

# 9. If Port 30005 is Occupied

Stop dump1090 service:

```bash
sudo systemctl stop dump1090-mutability
```

Then run the command again.

---

# 10. rbfeeder Configuration

Configuration file:

```
/etc/rbfeeder.ini
```

Configuration:

```ini
[network]
mode=beast
external_port=30005
external_host=127.0.0.1
```

### Explanation

rbfeeder **no longer reads from RTL-SDR directly**.

Instead:

```
RTL-SDR → dump1090 → TCP → rbfeeder
```

---

# 11. Writing Flight Data to JSON

To store flight data locally:

```bash
dump1090-mutability \
--net \
--net-bo-port 30005 \
--net-bind-address 127.0.0.1 \
--write-json <directory> \
--write-json-every 60
```

Example:

```
--write-json /home/raspberry/adsb_logs
```

### Output

JSON files generated every **60 seconds**.

These can later be parsed with **Python scripts** to create:

* CSV files
* Long-term flight logs
* flight statistics

---

# 12. Viewing the Flight Tracker (AirNav)

Open the feeder station page:

```
https://www.airnavradar.com/stations/EXTRPI690392
```

Flights will only appear if **dump1090 is running**.

If dump1090 is not running:

* The page loads
* But **no aircraft appear**

---

# 13. Displaying Map View on a TV

To show the flight map on an external display:

### Step 1 — Open Browser

1. Click **Raspberry Pi icon** (top-left).
2. Go to **Internet**
3. Click **Chromium Browser**

---

### Step 2 — Open Station Page

Click the **Feeder Station bookmark**.

Or open manually:

```
https://www.airnavradar.com/stations/EXTRPI690392
```

---

### Step 3 — Optimize Display

1. Adjust **zoom level**
2. Click **FULLSCREEN button** on the bottom-right of the webpage

The flight tracker is now ready for **live display on the TV**.

---

# 14. Obtaining a Station ID

To obtain the **AirNav Station ID**, run:

```bash
sudo rbfeeder --showkey
```

This displays the **Sharing Key**.

Example:

```
0a8a436adc101b030e303db0a660b7cf
```

Open the claim page:

```
https://www.airnavradar.com/raspberry-pi/claim
```

Paste the key to receive a **Station ID**.

---

# 15. AirNav Station Details

### Feeder Station Page

```
https://www.airnavradar.com/stations/EXTRPI690392
```

### Login Credentials

Email:

```
airnav1090@gmail.com
```

Password:

```
adsb1090
```

---

# 16. Sharing Key (Reference)

CLI command:

```bash
sudo rbfeeder --showkey --no-start
```

Sharing Key:

```
0a8a436adc101b030e303db0a660b7cf
```

---

# 17. Useful dump1090 Commands

View all options:

```bash
dump1090-mutability --help
```

Common commands:

Run decoder:

```bash
dump1090-mutability --interactive
```

Run with networking:

```bash
dump1090-mutability --net
```

Run with JSON logging:

```bash
dump1090-mutability --write-json <directory>
```

---

# 18. Installation Notes

`rbfeeder` officially supports **32-bit OS**.

However, this system uses **64-bit Raspberry Pi OS Trixie**.

Installation workarounds were used from the documentation located in:

```
/home/raspberry/ADS-B Project/Documentation
```

Files used:

```
Raspberry Pi RBFeeder Guide.pdf
RBfeeder on Raspberry Pi OS Trixie.pdf
```

Installation method:

```bash
sudo ./install.sh
```

or

Double-click `install.sh` → **Execute in Terminal**

---

# 19. Contact for System Support

**B.E. ECE Section 01 — Class of 2026**

**2-Step Authentication or System Issues**
**1. Dharunprakash J**

Email

```
dharunprakashjayamurthy@gmail.com
```

Phone

```
+91 90808 67203
```

**Technical Issues ADS-B | Antenna | Logger Related**
**2. Aadharsh B**

Email

```
aadharshbalasaravanan@gmail.com
```

Phone

```
+91 84288 13087
```

---

# 20. Final Working Configuration

Final working pipeline:

```
ADS-B Aircraft Signal
        ↓
1090 MHz Antenna
        ↓
RTL-SDR Dongle
        ↓
dump1090-mutability
        ↓
TCP : 127.0.0.1:30005
        ↓
rbfeeder
        ↓
AirNav Radar Website
        ↓
Live Flight Tracking
```

# Logging:

```
dump1090 → JSON files → Python parsing → permanent logs
```

---

✅ **System is now capable of**

* Receiving aircraft signals
* Decoding ADS-B messages
* Feeding AirNav Radar
* Logging flight data for analysis
* Displaying live flights on a TV or monitor
