# project2
cs483 project 2 : techincal report writing


Project 2 is culminating exercise for the digital forensics portion of CS483 in which you will demonstrate your ability to synthesize raw data from a incident response investigation into a summarized report suitable for your supervisor and the "customer", a victim company in this scenario.  

## Requirements
- Follow the guidance in Chapter 16, _Report Writing_, of IR3e when you write your report.
- The report should be written in a GitHub supported markup language you are comfortable with {markdown, ascii doc, or reStructuredText}.  Submit your report as contributions to this repository.
- Include screen captures or tool output as necessary to support your analysis.
- Your report doesn't need to cover every detail of the investigation, but should focus on the timeline of actions that occured which put company data at risk.
- Whenever possible, corroborate your findings between different evidence sources.  For instance, if a file was transferred across the network and executed, you should have evidence from the network capture, event logs, registry and the filesystem.  
- Provide recommendations for network and host signatures that could be implemented to detect malicious activity in the future. (yara signatures)

## Evidence
- All evidence will be found on the server at `//server.cs483.eecs.net/cases/pwndCase`
- The captured sparse disk images can be browsed with AcessData FTK Imager, and mounted as an additional drive for follow-on analyis with other tools.  The forensic disk images are the files with the '.ad1' file extention.  
- Memory samples (`.mem`) can be analyzed with volatility or Redline.

## Scenario 

You are the team lead for the Johnson, Johnson, and Johnson LLP Incident Reponse and Retribution Team.  Your team has just finished collecting the evidence from an intrusion into a customer network, and the victim company is worried that intellectual property has been stolen.  You need to provide a report to your leadership and to the Chief Information Security Officer (CISO) for Precision Widgets of North Dakota.  Unfortunately, although your team collected all the data you asked them to collect, its now the weekend and the report is due first thing Monday morning.  You won't be able to ask your team any clarifying questions!!  The customer, PWND, is most concerned with identifying the lateral movement within their organization and the exfiltration of sensitive data.  

### Background:
Precision Widgets of North Dakota is a manufacturer of high tech precision aircraft and mobility parts primarily for government customers across the globe. PWND owns patents for several processes and designs which are the foundation of their brand and their most precious asset.

PWND was recently outbid on a contract to manufacture 150,000 Rascal scooter performance exhaust systems for the local government of Fort Lauderdale Florida. Due to PWND's previous success in this area and substantial cost advantages in the market PWND's CEO and founder Billy Honeydew immediately suspects foul play.

Your team is an incident response consultant from Johnson and Johnson and Johnson LLP tasked with discovering if there was data stolen from the PWND network, and if so how.

### Data from the team

Questions:

- Recon
	- What is the internal IP address of PWND-CEO? `192.168.64.136`
	- What is the internal IP address of PWND-ITADMIN? `192.168.64.137`
	- What is the internal IP address of PWNDDC01? `192.168.64.149`
	- What is the full name of the domain? `pwnd.local`
	- What is Billy Honeydew's Twitter handle (no @)? `PWND_BHoneydew`
	- What is the Twitter handle of the individual he talks to (no @)? `mystery_strngr`
	- What is the full link to the malware (unshortened)? http://precisionwidgetsnorthdakota.com/PrecisionWidgets.bat
	- What is the active time bias running on PWNDITADMIN? `240`
- Delivery
	- What is the time in UTC (MM/DD/YYYY HH:MM) of the initial malware delivery? `05/01/2017 07:54`
	- What is the time in UTC (MM/DD/YYYY HH:MM) of the payload download on PWND-CEO? `05/03/2017 01:16`
- Weaponization 
	- What type of payload is the malware (exactly as you'd see it in the builder)? `windows/meterpreter/reverse_http`
- C2
	- What is the IP of the C2 server? `138.68.64.108`
	- What destination port is being used by the malware (format port/protocol)? `4443/tcp`
	- What is the time in UTC (MM/DD/YYYY HH:MM) of the first connection of pwnd-ceo to the C2? `05/03/2017 01:19`
	- What is the time in UTC (MM/DD/YYYY HH:MM) of the first connection of pwnd-itadmin to the C2? `05/03/2017 02:11`
	- What is the time in UTC (MM/DD/YYYY HH:MM) of the first connection of pwnddc01 to the C2? `05/03/2017 02:20`
	- How many packets were transferred between PWND-CEO and the C2 IP? `23142`
- Installation
	- What PID is the malware running under on PWND-CEO? `1708`
	- What data stream of the payload on disk indicates where it came from? `Zone Identifier`
	- In the decoded payload instructions that would be run on a x86 system, what is the exact path defined in "DownloadData"? http://138.68.64.108:4443/$n
	- What is the name of the process on PWNDITADMIN that has executable injected code? What is the 2 byte hex string that indicates this to you? Answer in form processname,hexbytes    `TPAutoConnSvc.exe,4d5a`
	-  What is the size in bytes of the file uploaded by the hacker to PWNDITADMIN? Based upon the differences in the created, modified and accessed times what file operation do you theorize was used to put it there? Format: size,operation    `2916,copy`
- Actions on Objectives
	- How much data (round to nearest MB) was transferred from the system that data was exfiltrated from? 11
	- What is the filename of the staging tool used? 7.exe
	- What is the true application name of the staging tool? 7za.exe
	- What is the mistaken name the attacker named the staging tool before changing it? `USERSBHONEYDEW7.EXE`
	- What command had resulted in the domain admin credential being stored in memory? `runas /user:pwnd\pwnddc01 cmd`
	- What protocol was used to transfer payloads laterally (1 guess)? `SMB`
	- What is the filename used as a payload in lateral movement? `prec.bat`
	- What time was the payload transferred to 192.168.64.137? `05/03/2017 02:06`
	- What time was the payload transferred to 192.168.64.149? `05/03/2017 02:16`
	- What protocol was used to execute payloads remotely? `wmic`
	- Try to login to the malicious user's twitter account - what is the password (2 guesses)? Note: Do not tweet, delete tweets, delete the account, or otherwise ruin the challenge for others.  `SecretPass123`
	- Who is the bad guy in this case (1 try)? `cmaldonado`
	- What is the total number of files inside the exfiltration archive? `55`
	- What is the most concerning folder name that was exfiltrated? *Intellectual Property*
