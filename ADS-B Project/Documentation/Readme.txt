(A) INSTALLATION

rdfeeder software is compatible with 32-bit OS only, work-arounds that reliably execute the software for 64-bit OS are available on the internet. The following web-pages presented as PDF files were used for the set-up currently running on this raspberry-pi.
1. Refer to 
	1.1. Raspberry Pi RBFeeder Guide.pdf
	1.2. RBfeeder on Raspberry Pi OS Trixie.pdf
in the "/home/raspberry/ADS-B Project/Documentation" directory for installation of ADS-B rbfeeder on 64-bit OS

2. Use the "sudo ./install.sh" or double left-click the install.sh file and select EXECTUTE IN TERMINAL in case of failure of the 
"sudo bash -c "$(wget -O - http://apt.rb24.com/inst_rbfeeder.sh)"" CLI (Command Line Instruction).

(B) USAGE & CREDENTIALS

The raspberry-pi in use is registered to "https://www.airnavradar.com/" with the following Unique IDs 
1. UFS (Unique Feeder Station) Page <Visit on Web Browser>:
https://www.airnavradar.com/stations/EXTRPI690392

[Refer Image "1.png" --EXPECTED WEB PAGE]

2. The following AirNav Station Credentials are used for logging onto the airnav site to access UFS :
	1. airnav1090@gmail.com
	2. adsb1090

3. Raspberry Pi : Claim Page "https://www.airnavradar.com/raspberry-pi/claim"
Sharing Key : 0a8a436adc101b030e303db0a660b7cf
CLI can be used to identify the Sharing Key using Terminal: "sudo rbfeeder --showkey --no-start"

(C) USEFUL CONTACTS

Contact(s) for 2 Step-Authentication : B.E. ECE Section 01 - Class of 2026
	1. Dharunprakash J : dharunprakashjayamurthy@gmail.com, +91 90808 67203
	2. Aadharsh B : aadharshbalasaravanan@gmail.com, +91 84288 13087
