
## Unit 11 Submission File: Network Security Homework 

### Part 1: Review Questions 

#### Security Control Types

The concept of defense in depth can be broken down into three different security control types. Identify the security control type of each set  of defense tactics.

1. Walls, bollards, fences, guard dogs, cameras, and lighting are what type of security control?

    Answer:Physical security 


2. Security awareness programs, BYOD policies, and ethical hiring practices are what type of security control?

    Answer:administrative security

3. Encryption, biometric fingerprint readers, firewalls, endpoint security, and intrusion detection systems are what type of security control?

    Answer:Technical security

#### Intrusion Detection and Attack indicators

1. What's the difference between an IDS and an IPS?

        Answer:Intrusion Detection system (IDS) rlates to the traffic and the systems analysing and monitoring.
        Compare current network activity to threat databases to assist with the detection security behaviours
        > security policy violations, Malware and port scanners.

        -Intersion Prevention Systems (IPS) are located within the layer as firewalls; protection between both the outside world and the internal network. Is used to deny traffic, via the constaints of the security profile.    


2. What's the difference between an Indicator of Attack and an Indicator of Compromise?

        Answer: The digital evidence that a cyber incident has occurred; the audits security relates to the speculation of a network breach is a n Indicator of Compromise

        - The Indicator of Attack (IOA) is a pyhical evidence trail that a cyber attack has occured.

#### The Cyber Kill Chain

Name each of the seven stages for the Cyber Kill chain and provide a brief example of each.

1. Stage 1:Reconnaissance - to gain as much information on the target before committing to the attack
    - Passive recon, using phishing for information 
    - Active Recn, to gain access to protected areas via or bypassing router and firewalls.

2. Stage 2:Weaponisation- use of malware to infect devices to allow with DOS attacks, such as 
            - Botnets target networks are forced to accept commands from another computer which assist the command hardware to attack other computers
            - DDOS (distributed denial of service attacks) the system is flooded with data tracffic.
            - Malware this code is injected into the system eg logic bombs, worms, packet sniffers

3. Stage 3: Delivery; the attacker can use emails to send code. direct hacking into the open port.
            - the code malware is sent to the target through phishing 

4. Stage 4: Exploitation; hackers find the weakness in the systems, the host machine is infected via either a install of malware to allow command execution. The other is install malware from the internet to allow command execution.

5. Stage 5:Installation; this is either a web shell on a infected webserver. Allows backdoor access to the target allows the attacher to bypass sec controls.

6. Stage 6:Command and Control; allows the control of the encryption keys eg, remote access trojans allow remote access to the target. The attack phase allow consistant connection . The use of Hypertext Transfer Protocols /Security (HTTP&HTTPS) such as self-signed certificates or custom encryption.

7. Stage 7: Actions; this relates to how the hackers realises the goals of the attack. 
    - extracting ransome for the encryption codes 
    - 


#### Snort Rule Analysis

Use the Snort rule to answer the following questions:

Snort Rule #1

```bash
alert tcp $EXTERNAL_NET any -> $HOME_NET 5800:5820 (msg:"ET SCAN Potential VNC Scan 5800-5820"; flags:S,12; threshold: type both, track by_src, count 5, seconds 60; reference:url,doc.emergingthreats.net/2002910; classtype:attempted-recon; sid:2002910; rev:5; metadata:created_at 2010_07_30, updated_at 2010_07_30;)
```

1. Break down the Sort Rule header and explain what is happening.

   Answer:alert tcp $EXTERNAL_NET any -> $HOME_NET 5800:5820 is the Rule header alert is the action; tcp aplies the rule; External_Net is anything except HOME_NET; and creates an alert that applies to all TCP (Transmission Control Protocols) packets  looking at any ports  on the home network.

    - (msg:"ET SCAN Potential VNC Scan 5800-5820" this is the message that is printed by the alert.


2. What stage of the Cyber Kill Chain does this alert violate?

   Answer:  This is the reconnaissance 

3. What kind of attack is indicated?

   Answer: classtype:attempted-recon

Snort Rule #2

```bash
alert tcp $EXTERNAL_NET $HTTP_PORTS -> $HOME_NET any (msg:"ET POLICY PE EXE or DLL Windows file download HTTP"; flow:established,to_client; flowbits:isnotset,ET.http.binary; flowbits:isnotset,ET.INFO.WindowsUpdate; file_data; content:"MZ"; within:2; byte_jump:4,58,relative,little; content:"PE|00 00|"; distance:-64; within:4; flowbits:set,ET.http.binary; metadata: former_category POLICY; reference:url,doc.emergingthreats.net/bin/view/Main/2018959; classtype:policy-violation; sid:2018959; rev:4; metadata:created_at 2014_08_19, updated_at 2017_02_01;)
```

1. Break down the Sort Rule header and explain what is happening.

   Answer: alert tcp $EXTERNAL_NET $HTTP_PORTS -> $HOME_NET - this rule header is used to use a range of addresses to be able to designate address that are not in range.

2. What layer of the Defense in Depth model does this alert violate?

   Answer: Delivery

3. What kind of attack is indicated?

   Answer: classtype:policy-violation -SQL Injections. 

Snort Rule #3

- Your turn! Write a Snort rule that alerts when traffic is detected inbound on port 4444 to the local network on any port. Be sure to include the `msg` in the Rule Option.

    Answer: alert tcp any 

### Part 2: "Drop Zone" Lab

#### Log into the Azure `firewalld` machine

Log in using the following credentials:

- Username: `sysadmin`
- Password: `cybersecurity`

#### Uninstall `ufw`

Before getting started, you should verify that you do not have any instances of `ufw` running. This will avoid conflicts with your `firewalld` service. This also ensures that `firewalld` will be your default firewall.

- Run the command that removes any running instance of `ufw`.

    ```bash
    $ <sudo ufw status numbered>
    <sudo ufw delete allow 22/tcp>
    ```

#### Enable and start `firewalld`

By default, these service should be running. If not, then run the following commands:

- Run the commands that enable and start `firewalld` upon boots and reboots.

    ```bash
    $ <sudo apt update>
    $ <sudo apt install firewalld>
     <firewall-cmd --permanent --zone=whatever --add-service=http>
    ```

  Note: This will ensure that `firewalld` remains active after each reboot.

#### Confirm that the service is running.

- Run the command that checks whether or not the `firewalld` service is up and running.

    ```bash
    $ <sudo firewall-cmd --state>
    ```


#### List all firewall rules currently configured.

Next, lists all currently configured firewall rules. This will give you a good idea of what's currently configured and save you time in the long run by not doing double work.

- Run the command that lists all currently configured firewall rules:

    ```bash
    $ <sudo systemctl status firewalld>
    ```

- Take note of what Zones and settings are configured. You many need to remove unneeded services and settings.

#### List all supported service types that can be enabled.

- Run the command that lists all currently supported services to see if the service you need is available

    ```bash
    $ <$ service --status-all>

    ```

- We can see that the `Home` and `Drop` Zones are created by default.


#### Zone Views

- Run the command that lists all currently configured zones.

    ```bash
    $ <firewall-cmd --get-zones>
    ```

- We can see that the `Public` and `Drop` Zones are created by default. Therefore, we will need to create Zones for `Web`, `Sales`, and `Mail`.

#### Create Zones for `Web`, `Sales` and `Mail`.

- Run the commands that creates Web, Sales and Mail zones.

    ```bash
    $ <sudo firewall -cmd --new -zone=web>
    <sudo firewall -cmd --new -zone=sales>
    <sudo firewall -cmd --new -zone=mail>

    ```

#### Set the zones to their designated interfaces:

- Run the commands that sets your `eth` interfaces to your zones.

    ```bash
    $ <sudo firewall -cmd --zone=web --change -interface=eth1>
    $ <sudo firewall -cmd --zone=sales --change -interface=eth2>
    $ <sudo firewall -cmd --zone=mail --change -interface=eth3>
   
    ```

#### Add services to the active zones:

- Run the commands that add services to the **public** zone, the **web** zone, the **sales** zone, and the **mail** zone.

- Public:

    ```bash
    $ <firewall-cmd --permanent --zone=public --change-interface=eth1>
    <sudo firewall-cmd --zone=public --add-service=http --permanent>
    ```

- Web:

    ```bash
    $ <<firewall-cmd --permanent --zone=public --change-interface=eth2>
    ```

- Sales

    ```bash
    $ <firewall-cmd --permanent --zone=public --change-interface=eth3>
    ```

- Mail

    ```bash
    $ <firewall-cmd --permanent --zone=public --change-interface=eth3>
    $ 
    ```

- What is the status of `http`, `https`, `smtp` and `pop3`?

< firewall-cmd --reload>


#### Add your adversaries to the Drop Zone.

- Run the command that will add all current and any future blacklisted IPs to the Drop Zone.

     ```bash
    $ <firewall-cmd --permanent --add-rich-rule="rule family='ipv4' source address='192.168.0.11' reject">
    
    ```

#### Make rules permanent then reload them:

It's good practice to ensure that your `firewalld` installation remains nailed up and retains its services across reboots. This ensure that the network remains secured after unplanned outages such as power failures.

- Run the command that reloads the `firewalld` configurations and writes it to memory

    ```bash
    $ <firewall-cmd --runtime-to-permanent>
    ```

#### View active Zones

Now, we'll want to provide truncated listings of all currently **active** zones. This a good time to verify your zone settings.

- Run the command that displays all zone services.

    ```bash
    $ < firewall-cmd --list-all-zones>
    ```


#### Block an IP address

- Use a rich-rule that blocks the IP address `138.138.0.3`.

    ```bash
    $ <firewall-cmd --permanent --add-rich-rule="rule family='ipv4' source address='138.138.0.3' reject">
    ```

#### Block Ping/ICMP Requests

Harden your network against `ping` scans by blocking `icmp ehco` replies.

- Run the command that blocks `pings` and `icmp` requests in your `public` zone.

    ```bash
    $ <echo “1” > /ping/ipv4/icmp_echo_ignore_all>
    ```

#### Rule Check

Now that you've set up your brand new `firewalld` installation, it's time to verify that all of the settings have taken effect.

- Run the command that lists all  of the rule settings. Do one command at a time for each zone.

    ```bash
    $ <systemctl status firewalld>
    $ <firewall-cmd --zone=zone-web --list-sources>
    $ <firewall-cmd --zone=zone-sales --list-sources>
    $ <firewall-cmd --zone=zone-mail --list-sources>

    ```

- Are all of our rules in place? If not, then go back and make the necessary modifications before checking again.


Congratulations! You have successfully configured and deployed a fully comprehensive `firewalld` installation.

---

### Part 3: IDS, IPS, DiD and Firewalls

Now, we will work on another lab. Before you start, complete the following review questions.

#### IDS vs. IPS Systems

1. Name and define two ways an IDS connects to a network.

   Answer 1: The Intrusion detection systems 
   = via a network hub

   Answer 2:
    network tap is ahardware device 

2. Describe how an IPS connects to a network.

   Answer: Through the connect via a switch  via a VLAN to a listening port.

3. What type of IDS compares patterns of traffic to predefined signatures and is unable to detect Zero-Day attacks?

   Answer:Snort is an open source based IDS it recognises malicious network packets but is uable to dectect zero day attacks

4. Which type of IDS is beneficial for detecting all suspicious traffic that deviates from the well-known baseline and is excellent at detecting when an attacker probes or sweeps a network?

   Answer: Anomaly or Behaviour based -use Artifical Intelligence, statistical mosdels 

#### Defense in Depth

1. For each of the following scenarios, provide the layer of Defense in Depth that applies:

    1.  A criminal hacker tailgates an employee through an exterior door into a secured facility, explaining that they forgot their badge at home.

        Answer:Physical Human firewall


    2. A zero-day goes undetected by antivirus software.

        Answer: exploits potential software weakness and is a the vendor dev has zero days to patch and the security phase is Application and development patching security

    3. A criminal successfully gains access to HR’s database.

        Answer:Endpoint security - gained access due to social engineering 

    4. A criminal hacker exploits a vulnerability within an operating system.

        Answer: Application Secuity

    5. A hacktivist organization successfully performs a DDoS attack, taking down a government website.

        Answer: DNS based protection 

    6. Data is classified at the wrong classification level.

        Answer:Administrative contols 

    7. A state sponsored hacker group successfully firewalked an organization to produce a list of active services on an email server.

        Answer:Application security  DNS attack

2. Name one method of protecting data-at-rest from being readable on hard drive.

    Answer: Data encryption - 

3. Name one method to protect data-in-transit.

    Answer:Using HTTPS (Hypertext Transfer Protocol /secure )

4. What technology could provide law enforcement with the ability to track and recover a stolen laptop.

   Answer:Use of software eg LoJack. 

5. How could you prevent an attacker from booting a stolen laptop using an external hard drive?

    Answer:Encrypt data, root password - add kernal parameter <singleinit=/bin/bash>
    Disable booting from an external media in BIOS


#### Firewall Architectures and Methodologies

1. Which type of firewall verifies the three-way TCP handshake? TCP handshake checks are designed to ensure that session packets are from legitimate sources.

  Answer:Statefull inspection fire wall 

2. Which type of firewall considers the connection as a whole? Meaning, instead of looking at only individual packets, these firewalls look at whole streams of packets at one time.

  Answer:Statelees firewalls are generally less reigorous - based on static information such as source information 

3. Which type of firewall intercepts all traffic prior to being forwarded to its final destination. In a sense, these firewalls act on behalf of the recipient by ensuring the traffic is safe prior to forwarding it?

  Answer: Proxt - Application firwall works with TCP can intercept all packets 


4. Which type of firewall examines data within a packet as it progresses through a network interface by examining source and destination IP address, port number, and packet type- all without opening the packet to inspect its contents?

  Answer: Stateless packet inspection



5. Which type of firewall filters based solely on source and destination MAC address?

  Answer:Stateful 



### Bonus Lab: "Green Eggs & SPAM"
In this activity, you will target spam, uncover its whereabouts, and attempt to discover the intent of the attacker.
 
- You will assume the role of a Jr. Security administrator working for the Department of Technology for the State of California.
 
- As a junior administrator, your primary role is to perform the initial triage of alert data: the initial investigation and analysis followed by an escalation of high priority alerts to senior incident handlers for further review.
 
- You will work as part of a Computer and Incident Response Team (CIRT), responsible for compiling **Threat Intelligence** as part of your incident report.

#### Threat Intelligence Card

**Note**: Log into the Security Onion VM and use the following **Indicator of Attack** to complete this portion of the homework. 

Locate the following Indicator of Attack in Sguil based off of the following:

- **Source IP/Port**: `188.124.9.56:80`
- **Destination Address/Port**: `192.168.3.35:1035`
- **Event Message**: `ET TROJAN JS/Nemucod.M.gen downloading EXE payload`

Answer the following:

1. What was the indicator of an attack?
   - Hint: What do the details of the reveal? 

    Answer: 


2. What was the adversarial motivation (purpose of attack)?

    Answer: 

3. Describe observations and indicators that may be related to the perpetrators of the intrusion. Categorize your insights according to the appropriate stage of the cyber kill chain, as structured in the following table.

| TTP | Example | Findings |
| --- | --- | --- | 
| **Reconnaissance** |  How did they attacker locate the victim? | 
| **Weaponization** |  What was it that was downloaded?|
| **Delivery** |    How was it downloaded?|
| **Exploitation** |  What does the exploit do?|
| **Installation** | How is the exploit installed?|
| **Command & Control (C2)** | How does the attacker gain control of the remote machine?|
| **Actions on Objectives** | What does the software that the attacker sent do to complete it's tasks?|


    Answer: 


4. What are your recommended mitigation strategies?


    Answer: 


5. List your third-party references.

    Answer: 


---

© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.
