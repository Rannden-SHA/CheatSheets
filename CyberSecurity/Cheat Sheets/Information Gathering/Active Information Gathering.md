# Active Information Gathering

Propietario: Adrian Gisbert Cabello

### 1. Introducción a la Recopilación de Información Activa

La recopilación de información activa implica la interacción directa con los servicios del objetivo. Esto puede incluir técnicas como el escaneo de puertos, enumeración de servicios y más. Este proceso es crucial para identificar vulnerabilidades y puntos de entrada en el sistema objetivo.

### 2. Técnicas Comunes de Recopilación de Información Activa

### 2.1. Escaneo de Puertos

**Nmap**

- Escaneo básico de puertos:
    
    ```bash
    nmap -sS 1.1.1.1
    ```
    
- Detectar sistema operativo y versiones de servicios:
    
    ```bash
    nmap -A 1.1.1.1
    ```
    
- Escaneo de puertos específicos:
    
    ```bash
    nmap -p 22,80,443 1.1.1.1
    ```
    
- Escaneo de todos los puertos:
    
    ```bash
    nmap -p- 1.1.1.1
    ```
    

**Masscan**

- Escaneo rápido de puertos:
    
    ```bash
    masscan -p0-65535 1.1.1.1 --rate=1000
    ```
    

### 2.2. Enumeración DNS

**Interacción con un servidor DNS**

- Consultar registros A:
    
    ```bash
    dig megacorpone.com
    ```
    
- Consultar registros MX:
    
    ```bash
    dig mx megacorpone.com
    ```
    

**Automatización de Búsquedas**

- Script para enumerar subdominios:
    
    ```bash
    for sub in $(cat subdomains.txt); do dig $sub.megacorpone.com; done
    ```
    

**Fuerza Bruta de Lookups**

- Enumeración con `dnsrecon`:
    
    ```bash
    dnsrecon -d megacorpone.com -t brt -D /usr/share/wordlists/dnsmap.txt
    ```
    

### 2.3. Enumeración SMB

**Escaneo para el servicio NetBIOS**

- Enumerar recursos compartidos:
    
    ```bash
    smbclient -L //1.1.1.1 -N
    ```
    

**Scripts NSE de Nmap para SMB**

- Enumerar usuarios:
    
    ```bash
    nmap --script smb-enum-users.nse -p 445 1.1.1.1
    ```
    
- Enumerar recursos compartidos:
    
    ```bash
    nmap --script smb-enum-shares.nse -p 445 1.1.1.1
    ```
    

**CrackMapExec**

- Enumerar usuarios y recursos compartidos:
    
    ```bash
    crackmapexec smb 1.1.1.1 -u usuario -p password
    ```
    

### 2.4. Enumeración NFS

**Escaneo para NFS Shares**

- Enumerar recursos compartidos NFS:
    
    ```bash
    showmount -e 1.1.1.1
    ```
    

**Scripts NSE de Nmap para NFS**

- Escanear recursos NFS:
    
    ```bash
    nmap --script nfs-ls,nfs-statfs,nfs-showmount -p 2049 1.1.1.1
    ```
    

### 2.5. Enumeración SMTP

**Nmap**

- Enumerar usuarios SMTP:
    
    ```bash
    nmap -p 25 --script smtp-enum-users 1.1.1.1
    ```
    

**smtp-user-enum**

- Enumerar usuarios:
    
    ```bash
    smtp-user-enum -M VRFY -U usuarios.txt -t 1.1.1.1
    ```
    

### 2.6. Enumeración SNMP

**Escaneo para SNMP**

- Escaneo básico de SNMP:
    
    ```bash
    snmpwalk -v 2c -c public 1.1.1.1
    ```
    

**Nmap**

- Enumerar información SNMP:
    
    ```bash
    nmap -sU -p 161 --script=snmp-info 1.1.1.1
    ```
    

**Windows SNMP Enumeration**

- Enumerar con `snmp-check`:
    
    ```bash
    snmp-check 1.1.1.1 -c public
    ```
    

### 2.7. Herramientas y Técnicas Avanzadas

**Metasploit**

- Usar módulos de Metasploit para la enumeración:
    
    ```bash
    msfconsole
    use auxiliary/scanner/smb/smb_version
    set RHOSTS 1.1.1.1
    run
    ```
    

**DirBuster y Gobuster**

- Enumeración de directorios web:
    
    ```bash
    gobuster dir -u http://1.1.1.1 -w /usr/share/wordlists/dirb/common.txt
    ```
    

**WPScan**

- Enumerar vulnerabilidades en WordPress:
    
    ```bash
    wpscan --url http://1.1.1.1 --enumerate u
    ```
    

**Nikto**

- Escanear vulnerabilidades web:
    
    ```bash
    nikto -h http://1.1.1.1
    ```