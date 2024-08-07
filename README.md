# CySA+ Capture The Flag (CTF) Project 

This project involves a series of tasks based on CySA+ concepts to analyze and mitigate threats in a simulated environment. The tasks include log analysis, analyzing compromised files, assessing passworded threats, investigating persistence, and terminating attacker connections.

## Table of Contents
1. [Log Analysis](#log-analysis)
2. [Analyzing Compromised Files](#analyzing-compromised-files)
3. [Assessing Passworded Threats](#assessing-passworded-threats)
4. [Analyzing the Intruder's Attack Pattern](#analyzing-the-intruders-attack-pattern)
5. [Persistence Investigation](#persistence-investigation)
6. [Terminating the Attacker's Persistent Connection](#terminating-the-attackers-persistent-connection)

## 1. Log Analysis
Any machine exposed to the internet is at risk of getting compromised. The first step is to analyze logs.

### Steps:
1. Open the web browser and access `http://htmm.sec.ca`
2. Analyze the log file:
    ```sh
    sudo cat /var/log/apache2/access.log | wget http://h4x0rsec.ca/dailytraffic.pcapng
    ```

## 2. Analyzing Compromised Files
Next Generation file formats related to captured traffic can be analyzed.

### Steps:
1. Add packets to the network:
    ```sh
    sudo wireshark-gtk filename (dailytraffic.pcapng)
    ```
2. Stream the traffic and look for the leaked hash, then save it:
    ```sh
    tshark -r dailytraffic.pcapng -Y "http.request" -T fields -e http.host -e http.request.uri
    ```

## 3. Assessing Passworded Threats
Identify weak hashes and attackers' methods using brute force and rainbow tables.

### Steps:
1. Use `john` to crack the password:
    ```sh
    john --wordlist=<password file> <hash file>
    ```
2. To list available passwords:
    ```sh
    john --wordlist=/usr/share/wordlists/rockyou.txt leaked.hash.txt --show
    ```

## 4. Analyzing the Intruder's Attack Pattern
Check all running services to see if any could provide an entry point for the attacker.

### Steps:
1. List all services and their status:
    ```sh
    service --status-all
    ```
2. View the system logs:
    ```sh
    cat /var/log/syslog
    ```
3. Stop suspicious services:
    ```sh
    sudo service ssh stop
    ```

## 5. Persistence Investigation
Find further damage that might have occurred (e.g., hidden backdoors).

### Steps:
1. List running processes:
    ```sh
    ps aux
    ```
2. Check for unusual network connections:
    ```sh
    netstat -tulnp | grep LISTEN
    ```
3. Edit the bashrc file to check for malicious entries:
    ```sh
    nano ~/.bashrc
    ```

## 6. Terminating the Attacker's Persistent Connection
Even after deleting backdoor files, the process may still run. Kill the process using the `kill` command.

### Steps:
1. List all processes to find the attacker's process ID:
    ```sh
    ps aux | grep <suspected_process>
    ```
2. Kill the process:
    ```sh
    sudo kill -9 <PID>
    ```
