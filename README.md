# skkill-Apache-Safer
scans log files for signature of attacks in the webserver or via ssh, then blocks the hacker for one month

skkill blocks IP addresses automatically
==================================================
this program scans log files for specific patterns. If a pattern matches, then it looks for an IP address and blocks it by entering a DROP statement into iptables.

I use this program to protect my webserver and ssh access.

Example:
========
Hackers scan the internet for unconfigured PHPs:

104.248.227.214 - - [29/Nov/2020:14:07:02 +0100] "GET //phpMyAdmin/scripts/setup.php HTTP/1.1" 404 455 "-" "-"
104.248.227.214 - - [29/Nov/2020:14:07:02 +0100] "GET //phpmyadmin/scripts/setup.php HTTP/1.1" 404 455 "-" "-"
104.248.227.214 - - [29/Nov/2020:14:07:02 +0100] "GET //myadmin/scripts/setup.php HTTP/1.1" 404 455 "-" "-"
104.248.227.214 - - [29/Nov/2020:14:07:02 +0100] "GET //MyAdmin/scripts/setup.php HTTP/1.1" 404 455 "-" "-"

skkill scans for these patterns, if detected then the IP is immediately blocked for one month.

The search patterns can be freely entered into a file.

Overview: skkill blocks IP addresses automatically
==================================================
   this program scans log files for specific patterns. If a pattern matches, then it looks for an IP address and
   blocks it by entering a DROP statement into iptables.
   You can list all blocked IPs with the command:  iptables -L  or  iptables -L -n 

Configuration:
==============

   this program needs two files in the same path as skkill :
   1) "logFileNames" .... enter the log files to be scanned. Enter the full path. Each filename in a separate line
   2) "searchPattern" ... enter your search patterns into this file. Each pattern in a separate line
   3) optionally the file "noblockIPs" can be used to enter IP addresses which should be ignored (i.e. the local network).
      Enter one IP address by line. Enter only the beginning of an address to specify a group of addresses. I.e.: to ignore all 
      IP addresses of the local network, enter  192.168.

   for sample files you can uncomment the call to generateSampleFiles(). Then sample files will be generated. Edit them for your requirements.

Running skkill:
===============

   skkill do not need any parameters. Just start it in a console:

   ./skkill ... will start skkill. All output will be printed to the standard output.

   ./skkill & ... runs skkill as a daemon in the background. This is the normal operation.

   skkill must run as root

Logging:
========

    If the variable dologging is set to 1, then skkill writes all activities into the file skkill.log

Compiling:
==========

   In the console enter this command line:
   cc skkill.c -o skkill

Additional Information:
=======================

   The file "blockedIPs" stores the blocked IP addresses and the blocking time (hours). 
   DO NOT modify this file ! 
   But it may be useful to read its contents:
   The first number is the blocking time in hours, followed by the IP address.
   
