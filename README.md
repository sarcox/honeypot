# CodePath Week 9 Write-up

For this assignment, I created several VM's using the AWS infastructure. 
+ MHN Admin - ec2-54-183-118-170.us-west-1.compute.amazonaws.com - 54.183.118.170

Aside from the MHN Admin VM, each VM was named after the type of honeypot it offered:
+ Conpot -ec2-52-53-148-137.us-west-1.compute.amazonaws.com - 52.53.148.137
+ Dionaea - ec2-54-153-60-139.us-west-1.compute.amazonaws.com - 54.153.60.139
+ Suricata - ec2-54-241-191-4.us-west-1.compute.amazonaws.com - 54.241.191.4

# AWS Setup
The setup in AWS ran very smootly.  The free-tier AWS account offered the Ubuntu 14.04 (trusty) with the micro sizing. 
I updated the Security Groups as indicated in the lab instructions:
<img src="https://github.com/sarcox/honeypot/blob/master/AWS-security-groups.png">

I followed the on-screen instruction to create a new key-pair and download my private key file. I found the Linux Academy tutorial on <a href="https://www.youtube.com/watch?v=bi7ow5NGC-U">SSH'ing to an EC2 instance via putty</a> very helpful. 

Once the system was setup, the instructions in Milestone 2: Install the MHN Admin Application were very easy to follow.

Next, I created a VM for the Dionaea Honey Pot. I took advantage of AWS's "Launch More like This" feature. I was basically able to replicate my original VM but tweak the Security Gruop settings to open all of the ports. Next, I used the wget call listed on the Deploy page of the MHN Server to finalize the configuration.

Later, I repeated these steps to setup the remaining VMs for Conpot and Suricata.

To test the Dionaea system, I ran nmap from a locally hosted linux VM I ran from Virtual Box. I used the command nmap 54.153.60.139.

As nmap ran, I saw the attack behavoir in the Attack screen of the MHN server.
<img src="https://github.com/sarcox/honeypot/blob/master/HoneyPot_itsMe.png">

As expected, most of the results were from my nmap scan. As a sanity check, I confirmed the public-facing IP of my locally hosted linux VM:
<img src="https://github.com/sarcox/honeypot/blob/master/WhatIsMyIP.png">

Once the scan was complete, I confirmed the open ports on the Dionaea system:
<img src="https://github.com/sarcox/honeypot/blob/master/nmap_results.png">

After a short time, I observed actual attacker activity coming from IPs located in China, Russion, North Korea, and the US!
<img src="https://github.com/sarcox/honeypot/blob/master/HoneyPot.png">

# Summary of Data Collected
<To Do>
  
