# IP2Location LITE Geo Metrics  


This app adds Geo based metering to Trisul using the IP2Location LITE databases. 

The following CSV databases are processed. The databases can be found at https://lite.ip2location.com/

1. ASN-LITE - for Autonomous System Number metrics 
2. DB3-LITE - for Country and City 
3. P2-LITE  - for Proxies 


## Installing 

To install this APP logon as admin, then select APP from _Web Admin > Manage > Apps._

Post install run the following 3 steps to keep the FireHOL list updated. 


### 1. Download the latest IP2 Location Databases

From https://lite.ip2location.com/ download the following databases into  `/usr/local/share/trisul-probe/plugins`

````bash
-rw-rw-r-- 1 guru guru  9082502 May 17 15:14 IP2LOCATION-LITE-ASN.CSV.ZIP
-rw-rw-r-- 1 guru guru 37129729 May 17 15:14 IP2LOCATION-LITE-DB3.CSV.ZIP
-rw-rw-r-- 1 guru guru  1000038 May 17 15:15 IP2PROXY-LITE-PX2.CSV.ZIP

````


### 2. Compile the lists  

For this you have to install LuaJIT on the probe.  For Ubuntu it is `sudo apt install luajit` 

Then run the following command to compile the databases. We are compiling twice because of the way Trisul
uses pipelines. Pipe0 will work with the 0.level and Pipe1 with 1.level. This increases performance and
concurrency 

````bash

luajit /usr/local/share/trisul-probe/plugins /usr/local/share/trisul-probe/plugins/trisul-ip2loc-0.level
luajit /usr/local/share/trisul-probe/plugins /usr/local/share/trisul-probe/plugins/trisul-ip2loc-1.level

````

This app automatically refereshes the list every hour. 


### 3.  How updates are picked up

Everytime you download and recompile, Trisul automatically picks up the changes. So you can setup a CRON for this.


### 4. Restart probe

Login as admin , go to Context : default > Admin Tasks > Start/Stop Tasks. Restart the probe


## Viewing data 

This app creates four new counter groups. Go to Retro > Counter to start your analysis.


## Notices

All sites, advertising materials and documentation mentioning features or use of this database must display the following acknowledgment:

"This site or product includes IP2Proxy LITE data available from https://www.ip2location.com/proxy-database."
"This site or product includes IP2Proxy LITE data available from https://www.ip2location.com/asn-database."


UPDATES
=======

````
0.0.1		May 22 2018			Initial release 
````


