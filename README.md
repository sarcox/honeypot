# CodePath Week 9 Write-up

For this assignment, I created several VM's using the AWS infastructure. 
+ MHN Admin - ec2-54-183-118-170.us-west-1.compute.amazonaws.com - 54.183.118.170

Aside from the MHN Admin VM, each VM was named after the type of honeypot it offered:
+ Conpot -ec2-52-53-148-137.us-west-1.compute.amazonaws.com - 52.53.148.137
+ Dionaea - ec2-54-153-60-139.us-west-1.compute.amazonaws.com - 54.153.60.139
+ Suricata - ec2-54-241-191-4.us-west-1.compute.amazonaws.com - 54.241.191.4

*Note:* I documented the [Setup process for AWS](./Week%209%20Project_%20Honeypot%20with%20AWS.pdf) for others in the course.

# AWS Setup
The setup in AWS ran very smootly.  The free-tier AWS account offered the Ubuntu 14.04 (trusty) with the micro sizing. 
I updated the Security Groups as indicated in the lab instructions:
<img src="https://github.com/sarcox/honeypot/blob/master/AWS-security-groups.png">

I followed the on-screen instruction to create a new key-pair and download my private key file. I found the Linux Academy tutorial on [SSH'ing to an EC2 instance via putty](https://www.youtube.com/watch?v=bi7ow5NGC-U) very helpful. 

Once the system was setup, the instructions in Milestone 2: Install the MHN Admin Application were very easy to follow.

Next, I created a VM for the Dionaea Honey Pot. I took advantage of AWS's *Launch More like This* feature. I was basically able to replicate my original VM but tweak the Security Gruop settings to open all of the ports. Next, I used the wget call listed on the Deploy page of the MHN Server to finalize the configuration.

Later, I repeated these steps to setup the remaining VMs for Conpot and Suricata.

To test the Dionaea system, I ran nmap from a locally hosted linux VM I ran from Virtual Box. I used the command nmap 54.153.60.139.

As nmap ran, I saw the attack behavoir in the Attack screen of the MHN server. (Plus 1 attack from an outside source so soon after deploying the honeypot!). 
<img src="https://github.com/sarcox/honeypot/blob/master/HoneyPot_itsMe.png">

As expected, most of the results were from my nmap scan. As a sanity check, I confirmed the public-facing IP of my locally hosted linux VM:
<img src="https://github.com/sarcox/honeypot/blob/master/WhatIsMyIP.png">

Once the scan was complete, I confirmed the open ports on the Dionaea system:
<img src="https://github.com/sarcox/honeypot/blob/master/nmap_results.png">

After a short time, I observed actual attacker activity coming from IPs located in China, Russion, North Korea, and the US!
<img src="https://github.com/sarcox/honeypot/blob/master/HoneyPot.png">

# Issues
After running for a few days, the Dionaea honeypot did not collect any malware samples. There were attacks detected, but no payloads. On invesitgation it seemed like the SMB wasn't running. I adjusted the Security Groups to explicitly allow UDP traffic on port 445.

I used this refererence to troubleshoot: http://ly0n.me/2017/08/17/collect-windows-malwarethreat-intelligence-with-cowrie-honeypot-pestudio/. My configuration didn't match what was written, which points to my incorrectly setting up the honey pot. I installed the honeypot manually, rather than using the wget command from the lab write-up. Once I did this, my configuration matched what was described and the SMB service was running. 

I intend to leave this honey pot running for another day or so to see if it will collect anything.

# Summary of Data Collected
The honeypots were up for 3 days and 8 hours. In that time, there were a total of 4797 events logged by the 3 honeypots (total was actually 7534, but I am subtracting out the 2737 events from the nmap scan I initiated for testing). Please see [sessions.json](https://github.com/sarcox/honeypot/blob/master/session.json) for full details.

The Suricata honeypot received the most attacks, at 866 attacks.

The top attacked ports were:
1. 1433 (249 times)
1. 22 (136 times)
1. 3306 (80 times)
1. 23 (23 times)
1. 8545 (17 times)

